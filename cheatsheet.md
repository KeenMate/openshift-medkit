# OpenShift CheatSheet for those who want to stay sane (based on v4.10)

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

Few notes:
- Application is not a real object, it just represents whole stack of OpenShift objects like image stream, build, deploy configuration and so on that are created for the application
- While application is being created it first looks for _Dockerfile_, if that is not found, it tries to determine if the source code can be build with _s2i_ (source to image) builders
- Source code folder in the repository can be also defined with ``--context-dir`` that is relative to the root of the repository, it can but also doesn't have to start with slash
- Use of ``--as-deployment-config`` parameter is necessary to create a deployment configuration for this new application
- Parameters below can be combined to get proper source code, for example branch and context dir

| Description  | Command | Example | Comment |
| -----------  | ------- | ------- | ------- |
| New app from local source | oc new-app /path/to/source/code --as-deployment-config |||
| New app from Git repository | oc new-app [git_repository_path] --as-deployment-config | oc new-app https://github.com/sclorg/cakephp-ex --as-deployment-config | This repository is build with PHP s2i, source code is taken from root folder |
| New app from Git repository from custom folder | oc new-app [git_repository_path] --context-dir=[relative_directory_path] --as-deployment-config | oc new-app https://github.com/sclorg/s2i-ruby-container.git --context-dir=2.0/test/puma-test-app --as-deployment-config | This repository is build with Ruby s2i |
| New app from Git repository with specific branch | oc new-app [git_repository_path]#[branch_name] --as-deployment-config | oc new-app https://github.com/not-existant/repo#dev --as-deployment-config | |
| New app from Git repository with custom name | oc new-app  [git_repository_path] --as-deployment-config --name=[custom_app_name] | oc new-app https://github.com/sclorg/cakephp-ex --as-deployment-config --name=cake ||
| New app from Git repository with custom labels | oc new-app  [git_repository_path] --as-deployment-config -l [label_name]=[custom_app_name] | oc new-app https://github.com/sclorg/cakephp-ex --as-deployment-config -l app=cake | Multiple labels are defined with multiple -l parameters|
| Get all objects for the app | oc get all -l app=[app_name] | oc get all -l app=cake | You can of course use any other label selector that you have defined, for example -l app_id=123 |
| Delete whole app | oc delete all -l app=[app_name] | oc delete -l app=cake | You can of course use any other label selector that you have defined, for example -l app_id=123 |


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
| Get and follow logs from pod's container | oc logs -f [pod_name] -c [container_name] | oc logs -f first-web-application-2-2wtqp -c elixir-app ||
| Delete pod | oc delete pod [pod_name] | oc delete pod first-web-application-2-2wtqp ||

## Services (exposing pods to inner world)

Few notes:
- Pods and deployConfigs can be exposed
- When exposing a service, service name is taken from pod or deployConfig name, unless overriden with ``--name=`` attribute
- There is no check if any of the pod's containers actually exposes given port
- You can expose given dc or pod multiple times, every time it gets a different IP address and service name
- You can expose multiple ports with the same service using ``--port [port_number],[port_number_2],[etc]``
- You can add custom attributes to exposed services with ``-l [attribute_name]=[attribute_value]``

| Description  | Command | Example | Comment |
| -----------  | ------- | ------- | ------- |
| Get help | oc explain service |||
| Get current services | oc get svc |||
| Get detailed service definition | oc get -o yaml svc/[service_name] | oc get -o yaml svc/first-web-application ||
| Exposing pod | oc expose pod/[pod_name] --port [port_number] | oc expose pod/first-world-application-pod --port 4000 ||
| Exposing pod with custom name | oc expose pod/[pod_name] --port [port_number] --name [service_name] | oc expose pod/first-world-application-pod --name=real_exposer ||
| Exposing service | oc expose [object_type (pod/dc/svc)]/[service_name] or oc expose service [service_name] | oc expose svc/first-world-application ||
| Exposing service with custom name | oc expose svc/[service_name] --name=[custom_service_name] | oc expose svc/first-world-application  ||
| Delete service | oc delete svc/[service_name] | oc delete svc/first-web-application ||


## Routes (exposing pods to outside world, by exposing services)

Few notes:
- Routes are used to expose HTTP(S) services to outside world and CANNOT be used for different protocols and applications, for example Redis, PostgreSQL and so on
- Routes exposes services to outside world
- When exposing a service with route, route name is taken from service name, unless overriden with ``--name=`` attribute
- Route name is combined with project base url into final address, for example _first-web-application-our-web.openshift.com_, unless overriden with ``--hostname=[different url address]`` parameter
- Route names are unique per project
- One service can be exposed by multiple routes with different names

| Description  | Command | Example | Comment |
| -----------  | ------- | ------- | ------- |
| Get help | oc explain route |||
| Get current routes | oc get route |||
| Get current routes with labels | oc get route --show-labels |||
| Expose service | oc expose svc/[service_name] or oc expose service [service_name] | oc expose svc/first-web-application ||
| Expose service with different name | oc expose svc/[service_name] --name=[new_route_name] | oc expose svc/first-web-application --name=fwa ||
| Expose service with different name | oc expose svc/[service_name] --hostname=[new_host_name] | oc expose svc/first-web-application --name=www.restaurant.com ||
| Delete service | oc delete route/[route_name] | oc delete route/first-web-application ||


## Replication controllers

Few notes:
- Replication controllers take care of creation and termination of pods based on desired replica number of pods

| Description  | Command | Example | Comment |
| -----------  | ------- | ------- | ------- |
| Get help | oc explain dc |||
| Get current replication controllers | oc get rc |||
| Watch replication controllers | oc get rc --watch |||
| Get replication controller details | oc describe rc/[rc_name] | oc describe rc/cakephp-ex-1 ||
| Get replication controller definition | oc get -o yaml rc/[rc_name] | oc get -o yaml rc/cakephp-ex-1 ||

## Deployment configuration

Few notes:
- Deployment configurations describe how one or multiple pods are deployed in OpenShift
- You can try dry run with --dry-run to see what will be deployed and how

| Description  | Command | Example | Comment |
| -----------  | ------- | ------- | ------- |
| Get help | oc explain dc |||
| Get current deployment config | oc get dc |||
| Get deployment config details | oc describe dc/[deploy_config_name] | oc describe dc/cakephp-ex | Here you can find exposed ports of each container, what triggers new builds, last build information, last events and so on||
| Get deployment config definition | oc get -o yaml dc/[deploy_config_name] | oc get -o yaml dc/cakephp-ex | This is a whole deployment configuration for whole application in yaml ||
| Start rollout of the latest version of code/configuration/settings | oc rollout latest dc/[deploy_config_name] | oc rollout latest dc/cakephp-ex ||
| Cancel ongoing rollout | oc rollout cancel dc/[deploy_config_name] | oc rollout cancel dc/cakephp-ex ||
| Rollback rollout to previous version | oc rollback dc/[deploy_config_name] | oc rollback dc/cakephp-ex | Disables automatic triggers for new builds, those have to be reenabled again |
| Rollback rollout to specific version | oc rollback dc/[deploy_config_name] --to-version=[version_number] | oc rollback dc/cakephp-ex --to-version=1 | Disables automatic triggers for new builds, those have to be reenabled again |
| Get rollouts history for dc | oc rollout history dc/[deploy_config_name] | oc rollout history dc/cakephp-ex ||
| Reset automatic triggers to auto | oc set triggers dc/[deploy_config_name] --auto | oc set triggers dc/cakephp-ex --auto ||
| Removes all triggers from dc | oc set triggers dc/[deploy_config_name] --remove-all | oc set triggers dc/cakephp-ex --remove-all | This effectivelly means all deployments have to be trigged manually |



## Builds


## Image streams/tags (inner mapping of docker images, for example to external sources like docker.io, quay.io and so on)

| Description  | Command | Example | Comment |
| -----------  | ------- | ------- | ------- |
| Get help for image streams | oc explain is |||
| Get existing image streams | oc get is |||
| Delete image streams | oc delete is/[image_stream_name] | oc delete is/first-web-application ||



## Image streams/tags

