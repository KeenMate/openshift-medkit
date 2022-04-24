# OpenShift CheatSheet for those who want to stay sane

> This is not a complete list, it's a list that gets you started and is divided to logical groups and chronological order as you would probably use OpenShift
>  
> [All commands in alphabetical order can be found here](https://docs.openshift.com/container-platform/4.10/cli_reference/openshift_cli/developer-cli-commands.html)



## Most common

| Description  | Command | Example | Comment |
| -----------  | ------- | ------- | ------- |
| Get help | oc explain [object_type] |||
| Get status | oc status | ||
| Get status | oc logs [object_type]/[resource_name] | oc logs deployment/elixir ||


## Projects

| Description  | Command | Example | Comment |
| -----------  | ------- | ------- | ------- |
| Get help | oc explain project |||
| Get current project | oc project | ||
| Get all projects | oc get project or oc projects (simplified list) | ||
| New project | oc new-project [project_name] | oc new-project first-web-application||
| Switch to project | oc project [other_project_name] | oc project second-web-application ||


> Once you select a project all commands below are run in that project context

## Config maps + Secrets

| Description  | Command | Example | Comment |
| -----------  | ------- | ------- | ------- |
| Get help | oc explain configMap and oc explain secret |||
| User created | oc create configmap dummy-config-ma |||


## Applications

## Pods

| Description  | Command | Example | Comment |
| -----------  | ------- | ------- | ------- |
| Get help | oc explain pod |||
| Get all pods | oc get pod or oc get pods | ||
| Get all pods with labels | oc get pod --show-labels |||
| Get all pods with selected labels as columns | oc get pod --label-columns=[comma_separated_list_of_label] | oc get pod --label-columns=deploymentconfig,app ||
| Get all pods with pod IP | oc get pods -o wide | ||
| Watch all pods | oc get pods --watch || Can be combined with _wide_ or other parameters |
| Get pod | oc get pod/[pod_name] | oc get pod/first-web-application-2-2wtqp ||
| Get pod | oc get pod [pod_name] | oc get pod first-web-application-2-2wtqp ||
| Get detailed pod definition | oc get -o yaml pod/[pod_name] | oc get -o yaml pod/first-web-application-2-2wtqp ||
| Get pod running details | oc describe pod/[pod_name] | oc describe pod/first-web-application-2-2wtqp ||
| Create pod from local file | oc create -f [file_path_to_pod_definition] | oc create -f pods/first-web-application.yaml ||
| Get into pod (into first container) | oc rsh [pod_name] | oc get pod first-web-application-2-2wtqp ||
| Get into pod and specific container | oc rsh [pod_name] -c [container_name] | oc get pod first-web-application-2-2wtqp -c elixir-app ||
| Run command in pod's first container | oc get pod [pod_name] [command] | oc get pod first-web-application-2-2wtqp ls /data ||
| Run command in pod's container | oc get pod -c [container_name] [pod_name] [command] | oc get pod -c elixir-app first-web-application-2-2wtqp ls /data ||
| Forward port temporarily to your local machine | oc port-forward [pod_name] [pod_port] | oc port-forward postgres-database-2-2wtqp 5432 ||
| Forward port temporarily to different port on your local machine | oc port-forward [pod_name] [local_port]:[pod_port] | oc port-forward postgres-database-2-2wtqp 15432:5432 ||
| Forward multiple ports temporarily to your local machine | oc port-forward [pod_name] [pod_port_1] [pod_port_2 and so on] | oc port-forward first-web-application-2-2wtqp 5432 8000 ||
| Forward multiple ports temporarily to different port on your local machine | oc port-forward [pod_name] [local_port_1:pod_port_1] [pod_port_2 and so on] | oc port-forward first-web-application-2-2wtqp 5432 9000:8080 ||
| Get logs from pod's first container | oc logs [pod_name] | oc logs first-web-application-2-2wtqp ||
| Get and follow logs from pod's first container | oc logs -f [pod_name] | oc logs -f first-web-application-2-2wtqp ||
| Get and follow logs from pod's container | oc logs [pod_name] -c [container_name] | oc logs first-web-application-2-2wtqp -c elixir-app ||
| Delete pod | oc delete pod [pod_name] | oc delete pod first-web-application-2-2wtqp ||

## Deployment config

## Builds

## Image streams/tags

