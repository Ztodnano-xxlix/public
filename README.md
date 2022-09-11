# Superalgos-Superalgos  
Skip to content
Search or jump to…
Pull requests
Issues
Marketplace
Explore
 
@ztodnano 
Your account has been flagged.
Because of that, your profile is hidden from the public. If you believe this is a mistake, contact support to have your account status reviewed.
ztodnano
/
Superalgos-Superalgos
Private template
Code
Issues
Pull requests
Discussions
Actions
Projects
Security
Insights
Settings
Superalgos-Superalgos
/
.github
/
workflows
/
aws.yml
in
master
 

Spaces

2

No wrap
1
# This workflow will build and push a new container image to Amazon ECR,
2
# and then will deploy a new task definition to Amazon ECS, when there is a push to the "master" branch.
3
#
4
# To use this workflow, you will need to complete the following set-up steps:
5
#
6
# 1. Create an ECR repository to store your images.
7
#    For example: `aws ecr create-repository --repository-name my-ecr-repo --region us-east-2`.
8
#    Replace the value of the `ECR_REPOSITORY` environment variable in the workflow below with your repository's name.
9
#    Replace the value of the `AWS_REGION` environment variable in the workflow below with your repository's region.
10
#
11
# 2. Create an ECS task definition, an ECS cluster, and an ECS service.
12
#    For example, follow the Getting Started guide on the ECS console:
13
#      https://us-east-2.console.aws.amazon.com/ecs/home?region=us-east-2#/firstRun
14
#    Replace the value of the `ECS_SERVICE` environment variable in the workflow below with the name you set for the Amazon ECS service.
15
#    Replace the value of the `ECS_CLUSTER` environment variable in the workflow below with the name you set for the cluster.
16
#
17
# 3. Store your ECS task definition as a JSON file in your repository.
18
#    The format should follow the output of `aws ecs register-task-definition --generate-cli-skeleton`.
19
#    Replace the value of the `ECS_TASK_DEFINITION` environment variable in the workflow below with the path to the JSON file.
20
#    Replace the value of the `CONTAINER_NAME` environment variable in the workflow below with the name of the container
21
#    in the `containerDefinitions` section of the task definition.
22
#
23
# 4. Store an IAM user access key in GitHub Actions secrets named `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`.
24
#    See the documentation for each action used below for the recommended IAM policies for this IAM user,
25
#    and best practices on handling the access key credentials.
26
- name: Setup Node.js environment
27
  uses: actions/setup-node@v3.4.1
28
  with:
29
    # Set always-auth in npmrc.
30
    always-auth: # optional, default is false
31
    # Version Spec of the version to use. Examples: 12.x, 10.15.1, >=10.15.0.
32
    node-version: # optional
33
    # File containing the version Spec of the version to use.  Examples: .nvmrc, .node-version, .tool-versions.
34
    node-version-file: # optional
35
    # Target architecture for Node to use. Examples: x86, x64. Will use system architecture by default.
36
    architecture: # optional
37
    # Set this option if you want the action to check for the latest available version that satisfies the version spec.
38
    check-latest: # optional
39
    # Optional registry to set up for auth. Will set the registry in a project level .npmrc and .yarnrc file, and set up auth to read in from env.NODE_AUTH_TOKEN.
40
    registry-url: # optional
41
    # Optional scope for authenticating against scoped registries. Will fall back to the repository owner when using the GitHub Packages registry (https://npm.pkg.github.com/).
42
    scope: # optional
43
    # Used to pull node distributions from node-versions.  Since there's a default, this is typically not supplied by the user.
44
    token: # optional, default is ${{ github.token }}
45
    # Used to specify a package manager for caching in the default directory. Supported values: npm, yarn, pnpm.
46
    cache: # optional
47
    # Used to specify the path to a dependency file: package-lock.json, yarn.lock, etc. Supports wildcards or a list of file names for caching multiple dependencies.
48
    cache-dependency-path: # optional
49
name: Deploy to Amazon ECS
50
​
51
on:
52
  push:
53
    branches:
54
      - "master"
55
​
56
env:
57
  AWS_REGION: MY_AWS_REGION                   # set this to your preferred AWS region, e.g. us-west-1
58
  ECR_REPOSITORY: MY_ECR_REPOSITORY           # set this to your Amazon ECR repository name
59
  ECS_SERVICE: MY_ECS_SERVICE                 # set this to your Amazon ECS service name
60
  ECS_CLUSTER: MY_ECS_CLUSTER                 # set this to your Amazon ECS cluster name
61
  ECS_TASK_DEFINITION: MY_ECS_TASK_DEFINITION # set this to the path to your Amazon ECS task definition
62
                                               # file, e.g. .aws/task-definition.json
63
  CONTAINER_NAME: MY_CONTAINER_NAME           # set this to the name of the container in the
64
                                               # containerDefinitions section of your task definition
65
​
66
permissions:
67
  contents: read
68
​
69
jobs:
70
  deploy:
71
    name: Deploy
72
    runs-on: ubuntu-latest
73
    environment: production
74
​
75
    steps:
76
    - name: Checkout
Use Control + Space to trigger autocomplete in most situations.
Getting started with a workflow
To help you get started, this guide shows you some basic examples. For the full GitHub Actions documentation on workflows, see "Configuring workflows."

Customizing when workflow runs are triggered
Set your workflow to run on push events to the main and release/* branches

on:
  push:
    branches:
    - main
    - release/*
Set your workflow to run on pull_request events that target the main branch

on:
  pull_request:
    branches:
    - main
Set your workflow to run every day of the week from Monday to Friday at 2:00 UTC

on:
  schedule:
  - cron: "0 2 * * 1-5"
For more information, see "Events that trigger workflows."

Manually running a workflow
To manually run a workflow, you can configure your workflow to use the workflow_dispatch event. This enables a "Run workflow" button on the Actions tab.

on:
  workflow_dispatch:
For more information, see "Manually running a workflow."

Running your jobs on different operating systems
GitHub Actions provides hosted runners for Linux, Windows, and macOS.

To set the operating system for your job, specify the operating system using runs-on:

jobs:
  my_job:
    name: deploy to staging
    runs-on: ubuntu-18.04
The available virtual machine types are:

ubuntu-latest, ubuntu-18.04, or ubuntu-16.04
windows-latest or windows-2019
macos-latest or macos-10.15
For more information, see "Virtual environments for GitHub Actions."

Using an action
Actions are reusable units of code that can be built and distributed by anyone on GitHub. You can find a variety of actions in GitHub Marketplace, and also in the official Actions repository.

To use an action, you must specify the repository that contains the action. We also recommend that you specify a Git tag to ensure you are using a released version of the action.

- name: Setup Node
  uses: actions/setup-node@v1
  with:
    node-version: '10.x'
For more information, see "Workflow syntax for GitHub Actions."

Running a command
You can run commands on the job's virtual machine.

- name: Install Dependencies
  run: npm install
For more information, see "Workflow syntax for GitHub Actions."

Running a job across a matrix of operating systems and runtime versions
You can automatically run a job across a set of different values, such as different versions of code libraries or operating systems.

For example, this job uses a matrix strategy to run across 3 versions of Node and 3 operating systems:

jobs:
  test:
    name: Test on node ${{ matrix.node_version }} and ${{ matrix.os }}
    runs-on: ${{ matrix.os }}
    strategy:
      matrix:
        node_version: ['8', '10', '12']
        os: [ubuntu-latest, windows-latest, macOS-latest]

    steps:
    - uses: actions/checkout@v1
    - name: Use Node.js ${{ matrix.node_version }}
      uses: actions/setup-node@v1
      with:
        node-version: ${{ matrix.node_version }}

    - name: npm install, build and test
      run: |
        npm install
        npm run build --if-present
        npm test
For more information, see "Workflow syntax for GitHub Actions."

Running steps or jobs conditionally
GitHub Actions supports conditions on steps and jobs using data present in your workflow context.

For example, to run a step only as part of a push and not in a pull_request, you can specify a condition in the if: property based on the event name:

steps:
- run: npm publish
  if: github.event_name == 'push'
For more information, see "Contexts and expression syntax for GitHub Actions."
