<img src="https://avatars1.githubusercontent.com/u/47143554?s=400&u=7c55eeec6479b4ff59df7cad452501a41635b0e4&v=4" alt="Cloudstate logo" width="200" align="right">
<br><br>
<br><br>
<br><br>

# Lab 0: Install the environment

## LAB Overview

#### This lab will help you to prepare the environment required for the rest of the workshop

## TASK 1: Create cluster
1. Open the console.cloud.google.com
2. On left menu choose Kubernetes Engine.
3. Click clusters and create one cluster with two nodes.

## Task 2: Install GCP SDK:
1. Open url: https://cloud.google.com/sdk/docs/quickstart-macos
2. Proceed with instruction and install SDK tools.

## Task 3: Configure credetials to kubernetes CLI
1. Install kubectl (K8s CLI) from instruction: https://kubernetes.io/docs/tasks/tools/install-kubectl/
2. Run command: <code>gcloud container clusters get-credentials [CLUSTER_NAME]</code>
3. Run command: <code>kubectl get nodes</code>
