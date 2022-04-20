#Usage

This command creates a build configuration named "hello-world-go" with labels and context-dir set to this folder

```
oc new-build https://github.com/KeenMate/openshift-templates.git --context-dir="demo/hello-world-go" --labels="IMAGE_NAME=hello-world-go" --name="hello-world-go"
```
