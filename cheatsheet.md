# OpenShift CheatSheet for those who want to stay sane

## Most common

| Description  | Command | Example |
| -----------  | ------- | ------- |
| Get status | oc status | |

## Projects

| Description  | Command | Example |
| -----------  | ------- | ------- |
| Get current project | oc project | |
| Get all projects | oc get project or oc projects (simplified list) | |
| New project | oc new-project [project-name] | oc new-project first-web-application|
| Switch to project | oc project [other-project-name] | oc project second-web-application |


> Once you select a project all commands below are run in that project context

## Config maps + Secrets

| Description  | Command | Example |
| -----------  | ------- | ------- |
| User created | oc create configmap dummy-config-ma | |


## Applications

## Pods

| Description  | Command | Example |
| -----------  | ------- | ------- |
| Get all pods | oc get pod | |
| Get all pods with labels | oc get pod --show-labels | |
| Get all pods with selected labels as columns | oc get pod --label-columns=[comma separated list of label] | oc get pod --label-columns=deploymentconfig,app |
| Get all pods with pod IP | oc get pods -o wide | |
| Get pod | oc get pod/[pod name] | oc get pod/hello-world-go-2-2wtqp |
| Get pod | oc get pod [pod name] | oc get pod hello-world-go-2-2wtqp |
| Get detailed pod view | oc get -o yaml pod/[pod name] | oc get -o yaml pod/hello-world-go-2-2wtqp |
| Create pod from local file | oc create pod | |




## Deploy config

## Builds

## Image streams/tags

