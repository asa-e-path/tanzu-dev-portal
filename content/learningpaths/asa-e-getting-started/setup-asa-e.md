---
date: '2022-10-06'
lastmod: '2022-10-06'
layout: single
team:
- Steven Pousty
- Jorge Morales Pou
title: Setup yourself to work with Azure Spring Apps
weight: 18
tags:
- Azure
- Spring
- Cloud Development
oldPath: ""
aliases: []
---

## Prerequisites

This guide assumes you know Spring, but you are not that familiar with Azure or Azure Spring Apps Enterprise (ASA-E). If you have limited experience with ASA-E we recommend you read a [basic document](https://onevmw-my.sharepoint.com/:w:/g/personal/spousty_vmware_com/EZq6t15kvZJEmM11jSrGYI0BNVg2ejUT-x9DRTHAZUOV9w?e=WQkBsF) basic Azure terminology. If you want to go deeper you can do the [course AZ-900T00](https://docs.microsoft.com/en-us/training/courses/az-900t00), especially the last 2 modules.

It is also important to understand that, for Azure Spring Apps, the web interface is primarily read-only. You can do some very basic high-level resource creation but the majority of your work has to happen through either the command line or IDE plugin ([VSCode](https://code.visualstudio.com/docs/azure/extensions) and [Intellij](https://plugins.jetbrains.com/plugin/8053-azure-toolkit-for-intellij)). Due to this architecture, there will not be many web screenshots until we get to features such as monitoring or logging.

There are separate how-to guides for Intellij and VScode that cover how to use the IDE plugins with Azure.

* [Intellij Azure Spring Apps](https://docs.microsoft.com/en-us/azure/spring-apps/how-to-intellij-deploy-apps) specific
* [Intellij general Azure usages](https://docs.microsoft.com/en-us/azure/developer/java/toolkit-for-intellij/)
* [VS Code Azure Spring Apps](https://code.visualstudio.com/docs/java/java-spring-apps) specific
* [VS Code general Azure usages](https://code.visualstudio.com/docs/azure/extensions)

We also assume you have a JVM > version 8 and Maven already installed on your machine. They should already be set up and working in any terminal where you plan to use the Azure CLI.

Finally, we also assume you have logged in Azure from the CLI or IDE.  For the CLI, if you top

```shell
$ az account show
```
If you have not logged in you should be prompted to login. If you are logged in, the response should be some JSON that contains your Subscription ID, Name, and other fields.

TODO[[[Put in a screenshot and text for intellij login]]]

With those preliminaries out of the way, let's deploy your Spring Application on ASA-E.

## Configure your system and create your work area in Azure Spring Apps

We are going to create just enough application that you can learn the terminology and basic mechanics of using ASA-E for your applications. In later modules we will go into more depth.

1. Login - because we can't open an iframe for the portal
   `az login --use-device-code`
   https://learn.microsoft.com/en-us/cli/azure/reference-index?view=azure-cli-latest#az-login

2. ResourceGroup
   ` az group create -l westus -n playing1`
   https://learn.microsoft.com/en-us/cli/azure/group?view=azure-cli-latest#az-group-create

3. Spring App Service
   `az spring create -n the-asa-service -g playing1 --sku Enterprise  --enable-gateway --enable-api-portal`
   https://learn.microsoft.com/en-us/cli/azure/spring?view=azure-cli-latest#az-spring-create
   Same as spinning up any service in Azure - in this case we are spinning up resources that know how to host Spring, and other language runtimes.
   Enterprise also makes it possible to add frequently used technology, such as an API portal, to the resources spun up.

This is kinda cool to set the defaults so you don't have to keep entereing it.
```shell
az configure --defaults \
    group=${RESOURCE_GROUP} \
    location=${REGION} \
    spring=${SPRING_APPS_SERVICE}
```