---
title: "Deploy Streamlit to Azure Container Apps with IAC and CICD - Overview"
layout: post
date: "2025-09-08"
permalink: posts/streamlit-azure-container-apps-overview/
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

Recently I have been practicing my data analysis skills with Python and I wanted to get more familiar with how to deploy a [Streamlit](https://streamlit.io/) data app to the cloud in a cost effective manner.

I will share how I went about this in this blog post.

In the end I successfully got the streamlit app deployed.

If you would like to check out the application before continuining with the post, take a look over at [https://streamlit.jarrodlilkendey.com](https://streamlit.jarrodlilkendey.com).

The code repository for this tutorial is available on my GitHub repo [streamlit-azure-container-app](https://github.com/jarrodlilkendey/streamlit-azure-container-app).

## Outlining the Approach

After evaluating a few options, I decided to go with a Azure Container Apps on a consumption billing plan which enables me to deploy a streamlit application to the cloud with the option of scaling to 0 when it is not in use, keeping cloud compute costs extremely minimal.

Before I could deploy the streamlit app to Azure Container Apps I needed to create a Docker image on the application.

After creating this Docker image I needed to host the Docker image in a location accessible through Azure, after looking at the prices on Azure Container Registry and comparing them to Amazon Elastic Container Registry, I ended up deciding to go with a public Amazon Elastic Container Registry repository for the application.

I wanted to use IAC to provisision the cloud infrastructure resources required, I wrote two Terraform workflow files to provision the public Amazon Elastic Container Registry repository and the Azure Container App.

I wanted to automate CICD of the streamlit application when a new push to master was made in the application's [GitHub repository](https://github.com/jarrodlilkendey/streamlit-azure-container-app), which I achieved using a GitHub Action workflow.

I wanted to point the subdomain "streamlit" on this domain (which is hosted on CloudFlare) to the Azure Container App and enable TLS to support the use of HTTPS for the application.

To achieve this I wrote some shell scripts that can automate this. The scripts utilise the CloudFlare API and the Azure CLI to adjust the DNS records in CloudFlare, download a CloudFlare origin certificate for the domain than bind the custom domain inside Azure Container Apps along with the TLS certificate.

## Related Posts

This post is part 1 of a [series](/categories/streamlit-to-azure-container-apps-with-iac-and-cicd/) of posts where I will write about my solution for getting a streamlit application deployed to Azure Container Apps with IAC and CICD.

See all of the posts included in this [series](/categories/streamlit-to-azure-container-apps-with-iac-and-cicd/) below.

{% assign category = "Streamlit to Azure Container Apps with IAC and CICD" %}

<ul>
  {% for post in site.categories[category] reversed %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
