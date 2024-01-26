+++ 
draft = false
date = 2023-09-15T10:51:47+12:00
title = "GitLab CI workflow with Python Flask app"
description = "An example of a GitLab CI workflow using a Python Flask web app"
slug = ""
authors = ["Srđan Đukić"]
tags = ["gitlab", "python", "flask", "gitlab ci", "deployments", "gcp"]
categories = []
externalLink = ""
series = []
+++
# GitLab CI  workflow with Python Flask app

## About

In this blog post we are going to learn about GitLab CI, specifically looking at Deploy/Release automation.

We will be using a simple [Python Flask](https://flask.palletsprojects.com/en/2.3.x/) app to do this, deploying to
[Google Cloud](https://cloud.google.com/)'s [Cloud Run service](https://cloud.google.com/run?hl=en).

The post assumes that you have a [GitLab](https://gitlab.com/) account (to store our code and run the CI), a [Google Cloud](https://cloud.google.com/) account (to have somewhere to deploy our code to) and [Python](https://www.python.org/) and [Docker](https://www.docker.com/) installed locally.

The code for this blog post can be found here: https://gitlab.com/srkiNZ/flask-gcp-demo

## First step - get a working app

As a first step, we are going to get a Python Flask app running locally (and also get a Docker container building
locally) to ensure that we have working code.

So, we first need to ensure that we have Python and pip installed if they are not already present. 

Next, create a directory to work in and setup the virtual environment:
```
mkdir flask-gcp-demo
cd flask-gcp-demo
python3 -m venv .
source ./bin/activate
```

Once that is done we install the pre-requisites with:
```
pip install flask gunicorn
```

and add the file "main.py" with the following contents:
```
import os

from flask import Flask

app = Flask(__name__)


@app.route("/")
def hello_world():
    """Example Hello World route."""
    name = os.environ.get("NAME", "World")
    return f"Hello {name}! Nice to meet you :-) again!"


if __name__ == "__main__":
    app.run(debug=True, host="0.0.0.0", port=int(os.environ.get("PORT", 8080)))
```

We can then test the above works by running:
```
python3 main.py
```
and then navigating to [http://localhost:8080/](http://localhost:8080).

If this is working, we can move onto the next step. If not, we will need to debug why it's not running.

Go ahead and stop the app running with Ctrl+C.

## Second step - Put the app into a container

So, in order to put the app into a container, firstly we should put the requirements into a file. We can do this with:
```
pip freeze > requirements.txt
```
This should result in a new "requirements.txt" file with the following contents:
```
Flask==3.0.0
Werkzeug==3.0.1
gunicorn==20.1.0
```
NOTE: The versions of packages might be different for your setup

Next, we will create a Dockerfile with the following contents:
```
FROM python:3.11-slim

# Allow statements and log messages to immediately appear in the logs
ENV PYTHONUNBUFFERED True

# Copy local code to the container image.
ENV APP_HOME /app
WORKDIR $APP_HOME
COPY . ./

# Install production dependencies.
RUN pip install --no-cache-dir -r requirements.txt

# Run the web service on container startup. Here we use the gunicorn
# webserver, with one worker process and 8 threads.
# For environments with multiple CPU cores, increase the number of workers
# to be equal to the cores available.
# Timeout is set to 0 to disable the timeouts of the workers to allow Cloud Run to handle instance scaling.
CMD exec gunicorn --bind :$PORT --workers 1 --threads 8 --timeout 0 main:app
```
Assuming that this build succeeds, we should have a Docker image with an ID output in the logs, for example:
```
=> => writing image sha256:b7ea714e556f36a21ea0754fa9031a0ca318a0e9f8a22a504470da02c9f59308
```

we can then run our Docker container locally with:
```
docker run -e PORT=8080 -p8080:8080 b7ea714e556f36a21ea0754fa9031a0ca318a0e9f8a22a504470da02c9f59308
```
and should be able to again open up http://localhost:8080 to see our app running, this time serving our app from inside
a running Docker container.

## Third step - Commit our code to GitLab and enable CI

Once we have some running code, let's [create a blank project in GitLab](https://docs.gitlab.com/ee/user/project/) (untick the "Initialize repository with a README" checkbox) and commit our code to it with:
```
git init --initial-branch=main
git remote add origin git@gitlab.com:[YOUR_USERNAME_HERE]/[YOUR_PROJECT_NAME_HERE].git
git add .
git commit -m "Initial commit"
git push --set-upstream origin main
```
Now that our code is safely in version control, we can create a [continuous integration](https://en.wikipedia.org/wiki/Continuous_integration) (CI)
pipeline to build  and deploy our application, in our case to Google Cloud's Cloud Run service.

For this step, we will be following the GitLab tutorial at: <https://about.gitlab.com/blog/2023/08/21/how-to-secure-cloud-run-deployment-with-auto-devops/>

NOTE: We are assuming that the reader is familiar with GitLabCI and building simple pipelines

We first need to add the Service Account Key, Project ID and Service ID to the list of CI variables to make it available to our build. Once that is done, we can add the following to a file ".gitlab-ci.yml":
```
deploy-job:
    stage: deploy
    image: google/cloud-sdk:latest
    script:
        - export GOOGLE_CLOUD_CREDENTIALS=$(echo $BASE64_GOOGLE_CLOUD_CREDENTIALS | base64 -d)
        - echo $GOOGLE_CLOUD_CREDENTIALS > service-account-key.json 
        - gcloud auth activate-service-account --key-file service-account-key.json 
        - gcloud config set project $PROJECT_ID 
        - gcloud auth configure-docker
        - gcloud builds submit --tag gcr.io/$PROJECT_ID/$SERVICE_ID
        - gcloud run deploy $SERVICE_ID --image gcr.io/$PROJECT_ID/$SERVICE_ID --region=australia-southeast2 --platform managed --allow-unauthenticated 
```
NOTE: Replace the region in the "gcloud run deploy ..." command above with the region that you are deploying to.

If the deployment fails with the following error message:
```
Deployment failed
ERROR: (gcloud.run.deploy) PERMISSION_DENIED: Permission 'iam.serviceaccounts.actAs' denied on service account 30704999140-compute@developer.gserviceaccount.com (or it may not exist).
```
it is likely due to the Service Account that GitLab CI is using not having the "Cloud Run Service Agent" IAM Role (as per [this SO issue](https://stackoverflow.com/questions/55788714/deploying-to-cloud-run-with-a-custom-service-account-failed-with-iam-serviceacco)).

If the deployment is successful, congratulations! We have just used GitLab CI to deploy our application to GCP's Cloud Run service!

In the next post, we will build on this work to set up separate environments to deploy to and explore GitLab CI's environment and automated deployment features.

