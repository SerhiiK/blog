---
title: "Poetry - easy python packaging and dependency management"
date: 2023-02-13T17:01:00+02:00
description: "How to use Poetry. Examples"
tags: ["python"]
ShowToc: true
TocOpen: true
cover:
  image: "/img/poetry.png"
  alt: "poetry_cover"
---

## Problem 

Python - one of the most popular programming language. It simple and easy to learn. 
Thats why Python has good ecosystem: you can find package for almost everything.
And this created a problem - how to manage this all packages on project ?

By default, packages installed in global Python env, that accessible to all projects.
This can be issue due to packages being dependent on specific version of other packages.
In result you will have dependency conflict.
The standart method to prevent dependency confict is to create a separate Python env for
each project. 

Python ecosystem has different tool for it.
Virtualenv - tool to create isolated Python environmnts. 
Pipenv - tool automatically creates and manages a virtualenv.
But for me these both tool seems like not modern. 
 
Therefore, my choice fell to Poetry. 

## Poetry 

[Poetry](python-poetry.org) is a tool for dependency management and packaging in Python. 
It allows you to declare the packages of your project's depends and will manage them for you. 
Poetry uses a lockfile to provide repeatable installs. 

Let's start example and I'll show how to use it. 

First of all we need to install Poetry. You can find [documentations](https://python-poetry.org/docs/#installation)
how to do it. If you are Linux user (WSL) or MacOS you may use this command. 
 
```
curl -sSL https://install.python-poetry.org | python3 -

```

You need also add Poetry to your PATH.

Don't forget to [enable tab completion](https://python-poetry.org/docs/#enable-tab-completion-for-bash-fish-or-zsh) for your shell.

## Basic usage

Let's create project with name 'pet-store'

```poetry new pet-store```

This will create pet-store folder with following structure:

```
pet-store
├── pet_store
│   └── __init__.py
├── pyproject.toml
├── README.rst
└── tests
    ├── __init__.py
    └── test_pet_store.py

```

As dependency we need Flask. Let's install it.

```poetry add Flask@2.2.2```

Poetry will download Flask and dependencies. List of all dependencies you can find
in file pyproject.yaml or call command `poetry show`.

## First run of app

Poetry creates a virtual environment for project thats why you need run your app
from this env. 
Befor run flask we need to create minimal app. Add this code in pet_store folder. 

<script>github</script>

For it you may use command `poetry run`. Try to run our flask in pet-store. 

```
poetry run flask --app pet_store/pet-store run
```

This command will start Flask server with our app. 

This is first commands what you need to know how to work with Poetry. This tool 
has good documentations and you will find what you need inside. 

