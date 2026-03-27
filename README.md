# GitHub Actions

This Repo should contain my notes and trainings on GitHub Actions<br/>
Initial Notes based on :

[GitHub Actions Masterclass: From Beginner to Advanced<br/>
LM Academy<br/>
Published by Packt Publishing<br/>
](https://learning.oreilly.com/course/github-actions-masterclass/9781837025411/)

## Intro to yaml
```yaml
key: "value"
another: value
number: 10
bool: true
object: 
    path: /some/path/to/file
    environment: prod
array: 
    - name : red
    - hex : ff0000
    - name: green
    - name: blue
```

> Be consistent in indentation.

## GitHub Actions Workflows, Jobs & Steps

* Workflows
    * Workflows are defined at repository level
    * Define which triggers start the workflow
    * One or more jobs
    * `.github/workflows` directory
* Jobs
    * Defined at the workflow level
    * Define which execution environment they are run
      `runs-on : ubuntu-latest`
    * One of more steps
    * Jobs run in parallel by default unless dependencies prevent
    * Jobs run in independent virtual machines
* Steps
    * Defined at Job level
    * Define script or GitHub Action to execute
    * Run sequentially by default


```yaml
name: My workflow
on: push

jobs:
    my-first-job:
        runs-on: ubuntu-latest
        steps:
            - run : echo "Hello world"
    My-second-job:
        runs-on: ubuntu-latest
        steps:
            - run: echo "Bye world"

```

```yml
# 01-building-blocks.yaml
name: 01 Building Blocks

on: push

jobs:
    echo-hello:
        runs-on: ubuntu-latest
        steps:
            - name: Echo Hello World
              run: echo "Hello, World!"
```

## Workflow events
[Events that trigger workflows](https://docs.github.com/en/actions/reference/workflows-and-actions/events-that-trigger-workflows
)
* Repository Events
    * push
    * issues
    * pull_request
    * pull_request_review
    * fork
    * ...
* Manual trigger 
    * Triggered via the UI
    * Triggered via an API call
    * Trifggered from another workflow
* Schedule
    * Run as a cron job

