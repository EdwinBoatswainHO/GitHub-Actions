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

```yaml
name: 02 - Workflow Events

on: 
    push:
    pull_request:

jobs:
    echo:
        runs-on: ubuntu-latest
        steps:
            - name: Show the trigger
              run: echo " I have been triggered by a(n) ${{ github.event_name }} event."
```

### Workflow Runners
* Github-hosted (standard)
    * Windows & Ubuntu
        * 2 cores 8 GB ram 14 GB Disk
    * Mac
        * 3 cores 14 GB ram 14 GB Disk
    * Managed Service
    * A VM is scoped to a job, so each job receives a clean VM instance
* Self-hosted
    * Run workflows on almost any infrastructure of your choice
    * All maintenance down to us
    * Can be added at the repo, organisation or enterprise level
    * Jobs do not necessarily have to run on clean instances
    * Do not use in public repos

### Pre-built Actions
```yaml
name: My NPM package workflow
on: push

jobs:
    node-20-releases:
        runs-on: ubuntu-latest
        steps:
            - uses: actions/setup-node@v3
              with:
                node-version: 20
            - run: npm ci
            - run: npm test
            - run: npm publish ...
```
> `actions/...` an official GitHut action

* This `uses` an existing action preventing duplication and mistakes.
* Configured `with` key-value pair
* Can be combined with other steps
* We can create own own for public or private use.
* Public actions are on the [marketplace](https://github.com/marketplace?type=actions)

See [04-using-actions.yaml](.github/workflows/04-using-actions.yaml)

### Event Filters & Activity Types

#### Event Filters

* Push
    * branches
    * branches-ignore
    * tags
    * tags-ignore
    * paths
    * paths-ignore
    * ...

```yaml
Name: My NPM package workflow
on:
  push:
    branches:
      - main
      - 'releases/**'
    paths-ignore:
      - 'docs/**'
...
```

See [Workflow syntax](https://docs.github.com/en/actions/reference/workflows-and-actions/workflow-syntax#onpushbranchestagsbranches-ignoretags-ignore)

#### Activity Types

Specify which types of triggers execute our workflow

* pull_request
    * opened
    * synchronise - new commit is pushed to HEAD ref of PR
    * closed
    * assigned
    * labeled
    * edited
    * ...

```yaml
name: My NPM package workflow
on:
  pull_request:
    types: [opened, synchronize]
    branches:
      - main
      - 'releases/**'
...
```

> Here the `branches` refer to the base branches we want to merge our code into.

See [Events that trigger workflows](https://docs.github.com/en/actions/reference/workflows-and-actions/events-that-trigger-workflows#pull_request)

## Working with Contexts
Access information about runs, variables, jobs and much more

Contexts
* github 
    * commit SHA
    * Event name
    * ref of branch or tag triggering the workflow
* env
    * Variables defined in a workflow, job or step
* inputs
    * input properties pass via `with` or to a manually triggered workflow
* vars
    * Custom config variables at organisation, repo and environment levels
* secrets
* matrix
* needs
* ...

See [Context Availability](https://docs.github.com/en/actions/reference/workflows-and-actions/contexts#context-availability)

To reference context use: `${{ <context> }}`

[06-contexts.yaml](./.github/workflows/06-contexts.yaml)

## Expressions and variables
### Expressions
* Can be used to reference information from multiple sources
* Must use the `${{ <expression>}}` syntax
* Can be a combination of 
    * literal values - strings, numbers, bools or null
    * context values
    * Builtin functions
* Support logical operators

```yaml
...
    steps:
      if: ${{ github.event_name}} == "push"
        run: |
          echo "Triggered by a push event"
          echo "Running on. ${{ github.ref_name}}"

...
```

### Variables
Variables are used to set and reuse non sensitive information.

Simgle Workflow

Hierachy
```
env
.
└──  job
    .
    └──  step
```
> Can be accessed with environment variable syntax
>
> `echo "Hello $NAME"`

Multiple Workflows

Hierachy
```
organisation
.
└── repository
    .
    └── environment
```
> Have to be accessed via the vars context 
>
> `echo "Hello ${{ vars.NAME }}"`

```yaml
  echo-prod:
    runs-on: ubuntu-latest
    # prod environment defined with GitHub UI
    environment: prod
    steps:
      - name: Print Variables
        run: |
        # Better to do this a the workflow level to avoid duplication
          eco "Of var: ${{ vars.UNDEFINED_VAR || 'default value' }}"
        ...
```
