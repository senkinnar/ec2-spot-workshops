---
title: "Cleanup"
date: 2021-08-07T08:30:11-07:00
chapter: false
weight: 200
---

{{% notice note %}}
If you're running in an account that was created for you as part of an AWS event, there's no need to go through the cleanup stage - the account will be closed automatically. \
If you're running in your own account, make sure you run through these steps to make sure you don't encounter unwanted costs.
{{% /notice %}}

## Removing the CloudFormation stack used for FIS
```
aws cloudformation delete-stack --stack-name $FIS_EXP_NAME
```

## Cleaning up HPA, CA, and the Microservice
```
cd ~/environment
kubectl delete hpa monte-carlo-pi-service
kubectl delete -f monte-carlo-pi-service.yaml
kubectl delete -f inflate-arm64.yaml
kubectl delete -f inflate-amd64.yaml
kubectl delete -f inflate-team1.yaml
kubectl delete -f inflate-spot.yaml
kubectl delete -f inflate.yaml

aws ecr delete-repository --repository-name monte-carlo-sim --force
```

## Removing the cluster, Managed node groups and Karpenter pre-requisites
```
cd ~/enviroment/eksworkshop
terraform destroy --auto-approve

aws cloudformation delete-stack --stack-name karpenter-workshop
aws cloudformation wait stack-delete-complete --stack-name karpenter-workshop
```

{{% notice tip %}}
If you get any error while running this command, perhaps it might be caused because the name you selected for your cloud9 environment is different from **karpenter-workshop**. You can either find out and replace the name in the commands with the right name or [Use the console to delete the environment](https://docs.aws.amazon.com/cloud9/latest/user-guide/delete-environment.html).
{{% /notice %}}
