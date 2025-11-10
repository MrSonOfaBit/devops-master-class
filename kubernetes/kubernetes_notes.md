# Course Kubernetes Notes
Kubernetes (short K8s) is an open source system for container orchestration. It helps you to automate all the steps you have to made in docker engine by yourself like deploying, scaling and managing containerized applications.

> ⓘ For the leaning course I use microK8s on a ubuntu wls. All examples and approaches are based on the microK8s system. See more under [MicroK8s documentation](https://microk8s.io/docs)


## Core Functions and benefits
Here are some kubernetes advantages explained in a short term.

1. **Automate Managing**: K8s helps you managing, scaling, deploying your applications. It is useful if you have a large environment of servers, multiple containers and services that you need zo manage and optimize
2. **Orchestrate containers**: You can group containers into logical groups called pods and manage them from cluster of virtual or physical machines
3. **Ensure application state**: Kubernetes will work on maintain your declared state for example having always a number of replicas running from special container
4. **Self-Managing**: If containers fail or nodes die, it can restart and reschedule containers

## Kubernetes Cluster
