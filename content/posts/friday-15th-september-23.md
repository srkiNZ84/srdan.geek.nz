+++ 
draft = true
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
[Google Cloud](https://cloud.google.com/).

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
Flask==2.1.0
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

## Third step - 
