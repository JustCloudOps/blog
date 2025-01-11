---
layout: post
title: "Be mindful when managing Terraform deployment roles."
date: 2025-01-11
---

Throughout the lifecycle of AWS infrastructure, you may need to adjust your deployment strategy, including making changes to the IAM roles used to deploy Terraform.

When handling the lifecycle of these roles, it's important to be mindful of several key resources that could be impacted:

#### EKS Clusters

The creator of an EKS cluster receives system:masters permissions to configure the cluster. If the aws_auth configmap becomes malformed, the Terraform deployment role can be used to recover the cluster. This recovery option, along with others, is discussed [here](https://repost.aws/knowledge-center/amazon-eks-cluster-access).

#### KMS Customer Managed Keys

The AWS KMS [documentation](https://docs.aws.amazon.com/kms/latest/developerguide/key-policy-default.html): directly calls out a concerning scenario:

> "...suppose you create a key policy that gives only one user access to the KMS key. If you then delete that user, the key     becomes unmanageable and you must contact AWS Support to regain access to the KMS key."


The appropriate solution for managing changes in Terraform deployment roles will vary by scenario. One approach is to associate and maintain a deployment role throughout the account's lifecycle. In situations where the impact is difficult to determine, it may be prudent to preserve the old role, which can then be assumed by the new role if an unmanageable resource is discovered.
