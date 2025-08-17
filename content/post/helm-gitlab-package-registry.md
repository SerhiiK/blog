---
title: "Using Gitlab Package Registry to store helm charts"
date: 2023-03-27T21:12:33+03:00
description: "How to store helm charts in Gitlab"
tags: ["helm","charts", "gitlab", "registry"]
ShowToc: true
TocOpen: true
---


GitLab Package Registry (GPR) is a powerful feature of GitLab that allows developers to store and share packages within their GitLab projects.<!--more-->
With this feature, developers can easily manage and distribute packages, such as Docker images, Maven artifacts, RubyGems, and npm packages,
without relying on external registries.
In this article, we will explore how to use GPR for storing Helm charts and which benefits you will have.

## Benefits

### Centralized Package Management
GPR helps to manage packages in one location.
Instead of relying on public packager registries in the situation with deleted packages. 

### Improved Security
GPR provides a private repository for storing packages and charts.
With this feature, you can keep charts secure and take access to only authorized members.

### Easy Collaboration
Storing helm a chart in GPR help to do a collaboration between members. GPR provides a panel where you
can see all charts, versions of charts, and other details of charts. 

## Using GPR for Helm charts
By default, Gitlab Package Registry is enabled. You should see it in the left menu. 

![menu](/images/gpr/menu.png)

If you don't have this menu you will need to enable GitLab Package Registry for your GitLab project. 
You can do this by navigating to your project's Settings > General > Visibility, project features, 
permissions  page and clicking on the  “Package registry" button.

After it, you need to create a token. 
- Go to your GitLab repository settings in the left-hand menu
- Click on "Repository"
- Expand "Deploy tokens"
- Enter the name of the token
- Select the scopes for the token 
  (*read_package_registry*,
   *write_package_registry*)
- Don't forget to save the token, as it will not be displayed again.

### Work with charts
Now we have all to push charts. If you don't have a chart let's create this:   
```helm create foo```  
Now we need to package  it:   
```helm package foo/```
Once built, a chart can be uploaded to the desired channel with curl.
```
curl --request POST \
     --form 'chart=@foo-0.1.0.tgz' \
     --user <username>:<access_token> \
     https://gitlab.example.com/api/v4/projects/<project_id>/packages/helm/api/<channel>/charts
```
where:
 - *username* - username for deploy token (by default gitlab+deploy-token-nnnn)
 - *access_token* - saved token *project_id* - the id of repository. You may find it in Project information. 
 - *channel* - some types of a branch. You may have "dev", "stable" branches. 

If all passed ok you will have a message: `{"message":"201 Created"}`

Now open *Package and registries > Package Registry*. You will see your helm chart.

![chart](/images/gpr/chart.png)

### Add helm repository to a local machine 
For adding repository use this command:
```
helm repo add --username <user> --password <pass> gitlab-helm \ 
https://gitlab.com/api/v4/projects/<project_id>/packages/helm/<channel>
```
