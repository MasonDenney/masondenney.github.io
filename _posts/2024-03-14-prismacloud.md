---
title: "Prisma Cloud and Bridgecrew"
layout: post
date: 2024-03-14 00:00
image: /assets/images/markdown.jpg
headerImage: false
tag:
- prisma cloud
- palo alto
- bridgecrew
- kubernetes
star: false
category: blog
author: masondenney
description: Prisma Cloud and Bridgecrew Training
---
# Prisma Cloud and Bridgecrew Training
## Prisma Cloud
Palo Alto Prisma Cloud is currently offering workshops for prospective customers at <https://www.paloaltonetworks.com/prisma/cloud-interactive>. These workshops start with a sales pitch and end with a hands-on self-guided workshop. The the instructions for the workshops are at <https://github.com/PaloAltoNetworks/prisma-cloud-bootcamps> which I forked into a personal repo at <https://github.com/MasonDenney/prisma-cloud-bootcamps>.

## Bridgecrew
Several years ago Palo Alto Networks acquired Bridgecrew. Bridgecrew created checkov as well as a few other tools. At the end of the workshop instructions you can find a list of vulnerable by design terraform repos by bridegecrew such as:
- [TerraGoat - Terraform (AWS/GCP/Azure) Infrastructure](https://github.com/bridgecrewio/terragoat)
- [Cfngoat - AWS Cloudformation Template](https://github.com/bridgecrewio/cfngoat)
- [CdkGoat - AWS CDK Infrastructure](https://github.com/bridgecrewio/cdkgoat)
- [BicepGoat - Azure (Bicep and ARM Infrastructure)](https://github.com/bridgecrewio/bicepgoat)
- [KubernetesGoat - KubernetesCluster](https://github.com/bridgecrewio/kubernetes-goattest)
- [KustomizeGoat - Kustomize deployment](https://github.com/bridgecrewio/kustomizegoat)
- [SupplyGoat - Supply Chain Analysis](https://github.com/bridgecrewio/supplygoat)

## Prisma Cloud Terraform Providers
You can find all PaloAltoNetworks Providers here: <https://registry.terraform.io/search/providers?q=PaloAltoNetworks>
- prismacloud = <https://registry.terraform.io/providers/PaloAltoNetworks/prismacloud/latest>
  - Targets something like api1.prismacloud.io
- prismacloud-waas = <https://registry.terraform.io/providers/PaloAltoNetworks/prismacloud-waas/latest>
  - Targets something like api1.prismacloud.io
- prismacloudcompute = <https://registry.terraform.io/providers/PaloAltoNetworks/prismacloudcompute/latest>
  - Targets something like https://us-west1.cloud.twistlock.com/us-1-123456789

## Prisma Cloud APIs
There are different APIs that have different URLs and schemas, enterprise docs at <https://pan.dev/prisma-cloud/api/>
- CSPM = <https://pan.dev/prisma-cloud/api/cspm/>
  - Targets something like api1.prismacloud.io
- CAS = <https://pan.dev/prisma-cloud/api/code/>
  - Targets something like api1.prismacloud.io
- CWPP = <https://pan.dev/prisma-cloud/api/cwpp/>
  - Targets something like https://us-west1.cloud.twistlock.com/us-1-123456789
  - used by twistcli to get defenders