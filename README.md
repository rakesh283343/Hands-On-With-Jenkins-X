# Introduction
On this meetup, we will walk you through creating a Jenkins X cluster on GKE, creating an application using a Jenkins X quickstart, pushing the application into a staging environment.

Name: Gareth Evans

Role: Principal Software Engineer, Cloudbees

Email: gevans@cloudbees.com

Titter: garethbryncyn

Github: https://github.com/garethjevans

# Requirements
* Google Cloud Shell
* Basic Linux cli knowledge

# Setup
TODO

* Hub CLI

# Further Reading
* GKE Documentation
* Jenkins X Documentation - https://jenkins-x.io/documentation/
* Jenkins X GitHub - https://github.com/jenkins-x
* Tekton Documentation - https://github.com/tektoncd/pipeline/tree/master/docs
* Prow - https://github.com/kubernetes/test-infra/tree/master/prow
* Next Generation Pipeline - https://www.cloudbees.com/blog/move-toward-next-generation-pipelines
* Serverless Jenkins with Jenkins X - https://medium.com/@jdrawlings/serverless-jenkins-with-jenkins-x-9134cbfe6870

# Download Dependencies

```
curl -L https://github.com/jenkins-x/jx/releases/download/v2.0.230/jx-linux-amd64.tar.gz | tar xzv
sudo mv jx /usr/local/bin
```

# Create Cluster

Ensure you are logged in:
```
$ gcloud auth login
```

Select the shared project, if you don’t have enough credit:
```
$ gcloud config set project jenkins-x-workshop-242909
```

Create a new GKE cluster and install Jenkins X:
```
$ jx create cluster gke --ng --skip-login -n my-name
```

Select the JX namespace
```
jx ns jx
```

View all deployments

```
$ kubectl get deployments
```

View all pods

```
kubectl get pods
```

Create an golang application

```
jx create quickstart -l Go
```

Watch the build logs

```
jx get build logs
```

Make a change to the application

Install the HUB Cli

TODO need instructions for CloudShell

```
brew install hub
hub version
```

```
cd <application>
git checkout -b wip
vi main.go
git add -A
git commit -m "feat: updated welcome message"
git push origin wip
hub pull-request
```

```
$ jx get environments
$ jx get pipelines
$ jx start pipeline
$ jx get build logs -w
$ jx get activity
$ jx create quickstart -l Go
$ jx get build logs -f <project-name>
$ jx get activity -f <project-name>
$ jx get applications
Create a Pull Request for your application:
$ git checkout -b my-branch
$ git commit -am “message” && git push origin my-branch
$ hub pull-request
View Preview Environments:
$ jx get preview
Promote to production environment:
$ jx get applications
$ jx promote my-app --version 0.0.1 --env production

Check whether the application was promoted:
$ jx get applications

# Cleanup
Delete the GKE cluster:
$ gcloud container clusters delete <cluster-name> --region europe-west1-b
Remove storage:
$ gsutil ls && gsutil -m rm -r gs://<cluster-name>-<bucket-name>
Remove service accounts:
$ gcloud iam service-accounts list && gcloud iam service-accounts delete <name>@<project>.iam.gserviceaccount.com
Cleanup the local configuration:
$ rm -rvf ~/.jx
```