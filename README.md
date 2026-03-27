# GitHub Actions

This Repo should contain my notes and trainings on GitHub Actions
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




