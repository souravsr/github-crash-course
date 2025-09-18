# Github Actions Notes
- Key elements/ componenets of Github Actions
    - Workflows:
        - attached to a GitHub repository
        - contains one or more Jobs
        - Triggered upon certain Events
    - Jobs:
        - denine a Runner (execution environment)
    - Steps:
- There are many templets available for use on the Github, we can either use those or even create new workflow from scratch
- Example of a workflow
    - Uses yaml file, so need to properlt indent as this works on indentation
    - name: name of the workflow (reserved keyword)

```yaml
#name of the workflow
name: First Workflow
#event to trigger the workflow
on:
  #workflow will trigger when a pull_request is opened
  pull_request:
    #types can be mentioned as below or using types: opened or [opened, other_events]
    types:
      #activty type is set to opened
      - opened
  #another event to trigger the workflow
  #NOTE: the syntax has to be this was even if there is no other line below
  workflow_dispatch:
    #even if no lines below, the event should have ':'
  #workflow will trigger workflow on push event
  push:
    #this filter makes sure only push to main branch will trigger the workflow
    branches:
      - main
      #- 'dev-*', 'feat/**' we can use this as well 
    paths-ignore:
      #worflow will trigger on push except any changes made at this path
      - '.github/workflow/*'

jobs:
  #name of the job
  first-job:
    #runner the job will use
    runs-on: ubuntu-latest
    #steps that would be performed by the job
    steps:
      #name of the step
      - name: Get Code
        #we are using existing official Action
        uses: actions/checkout@v3
      #name of second step  
      - name: Install NodeJS
        #another existing Actions
        uses: actions/setup-node@v3
        with:
          #specifying version to install
          node-version: 18
      - name: install dependencies
        run: npm ci
  #name of second job
  second-job:
    #If we need the job to run after result of any other job we can acheove that using "needs" keyword
    #this job will trigger after result from the first-job above
    #we can provide multiple jobs as well needs: [job1, job2, ...]
    needs: first-job
    #each job has its own runner
    runs-on: ububtu-latest
    steps:
      - name: step 1 of second job
        run: echo "Running step 1 of second job"
```
