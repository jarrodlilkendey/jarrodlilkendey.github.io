---
title: "Deploy Streamlit to Azure Container Apps with IAC and CICD - Containerizing the Application"
layout: post
date: "2025-09-09 11:00:00 +1000"
permalink: posts/streamlit-azure-container-apps-containerizing/
categories: [Streamlit to Azure Container Apps with IAC and CICD]
tags:
  [
    streamlit,
    azure,
    azure-container-apps,
    terraform,
    github-actions,
    cicd,
    iac,
    cloud,
    docker,
  ]
# img_path: /assets/images/position-images-next-to-div/
---

In this post, I will use Docker to containerize my streamlit application, which is a requirement in order to deploy my streamlit application to Azure Container Apps.

This post is part 3 of a [series](/categories/streamlit-to-azure-container-apps-with-iac-and-cicd/) of posts where I will write about my solution for getting a streamlit application deployed to Azure Container Apps with IAC and CICD.

The code repository for this tutorial is available on my GitHub repo [streamlit-azure-container-app](https://github.com/jarrodlilkendey/streamlit-azure-container-app).

## Containerizing a Streamlit Application with Docker

Azure Container Apps requires a container image in order to run your application.

So what we will do next is create a Docker container image out of the Python code that we have already written for the streamlit application.

### Creating a Dockerfile to Build a Container Image

We will start by creating a Dockerfile.

The Dockerfile provides the instructions describing how to assemble the container image of your application which includes things such as:

1. The base image it will start from
1. Any files to copy over
1. Commands to run
1. Ports to expose

See the complete Dockerfile for the streamlit application below.

```Dockerfile
FROM python:3.9-slim

WORKDIR /app

COPY requirements.txt /app
RUN pip3 install -r requirements.txt

COPY . /app

EXPOSE 8501

HEALTHCHECK CMD curl --fail http://localhost:8501/_stcore/health

ENTRYPOINT ["streamlit", "run", "app.py", "--server.port=8501", "--server.address=0.0.0.0"]
```

### Keeping Unwanted Files out of the Docker Container Image

You want to make sure files from within your project that aren't needed in the container image aren't copied over.

This is mainly for two reasons:

1. You don't want to make the container image any bigger than what it needs to be
1. You want to keep any sensitive files out of your container image to prevent security incidents

The COPY command in the Dockerfile above will ignore the files and file patterns that are specified in the `.dockerignore` file.

See the .dockerignore file I used for my streamlit application below.

{: file='.dockerignore'}

```text
/.venv
README.md
/iac
/.github
/certs
```

### Building and Running the Docker Container Image Locally

We are now ready to build the container image with Docker then run it locally as a Docker container.

You will need to have Docker running on your machine to complete this part of the tutorial.

In the root project directory containing the Dockerfile and the .dockerignore file use the following commands to build and run the Docker container image locally.

```
docker build -t streamlit .
docker run -p 8501:8501 streamlit
```

The application should be accessible in the browser on localhost on port 8501.

## Related Posts

This post is part 3 of a [series](/categories/streamlit-to-azure-container-apps-with-iac-and-cicd/) of posts where I will write about my solution for getting a streamlit application deployed to Azure Container Apps with IAC and CICD.

See all of the posts included in this [series](/categories/streamlit-to-azure-container-apps-with-iac-and-cicd/) below.

{% assign category = "Streamlit to Azure Container Apps with IAC and CICD" %}

<ul>
  {% for post in site.categories[category] reversed %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
