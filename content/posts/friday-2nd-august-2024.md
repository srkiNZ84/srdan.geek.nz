+++ 
draft = false
date = 2024-08-02T18:38:39+12:00
title = "Creating new Hugo blog posts"
description = "A description of how to create new blog posts"
slug = ""
authors = ["Srđan Đukić"]
tags = ["hugo", "blog"]
categories = []
externalLink = ""
series = []
+++
# How I create new blog posts

So, this is in a way a "meta" article about how I create new blog posts. It's me mostly being kind to my future self by
documenting how to create a new blog post.

## The command to create the post

The command to create the new post is:

```
hugo new content/posts/[NAME_OF_FILE_HERE].md

# e.g. hugo new content/posts/friday-2nd-august-2024.md
```
from the help description it works by being able to guess the content "kind" based on the path:
```
% hugo new --help
Create a new content file and automatically set the date and title.
It will guess which kind of file to create based on the path provided.
```

## Add the content

Now we can create the content by editing the created file:
```
vim content/posts/[NAME_OF_FILE_HERE].md 
```
In order to add images to the post and link them, we have to put the images we want to reference under the `content/public/static` directory. Once that is done, we can reference them using a relative path like:
```
![some alt text here](../../static/name_of_image.png)
```

## Review using a local server

Start up a local server to build the pages and be able to access it locally with:
```
hugo server -D
```
You should now be able to view the new content on the site at <http://localhost:1313/>.

NOTE: If the post is marked as a "Draft" you might need to run:
```
hugo server --buildDrafts
```

## Publish the new content

Assuming that you are happy with the content, you can publish it to the git repo at GitHub and have it published using
GitHub Pages + Actions:
```
git add content/posts/*
git ci -m "Adding new post"
git push
```
The definition for how this works can be found here:
<https://github.com/srkiNZ84/srdan.geek.nz/blob/main/.github/workflows/gh-pages.yml>

This will publish the static content to the "gh-pages" branch of the git repository which is what GitHub
Pages looks at when loading the public site at <https://srdan.geek.nz>.
