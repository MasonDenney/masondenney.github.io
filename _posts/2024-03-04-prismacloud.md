---
title: "Prisma Cloud and Bridgecrew"
layout: post
date: 2024-03-04 00:00
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
description: Prisma Cloud and Bridgecrew
---
# Prisma Cloud and Bridgecrew
## Prisma Cloud
Palo Alto Prisma Cloud is currently offering workshops for prospective customers at <https://www.paloaltonetworks.com/prisma/cloud-interactive>. These workshops start with a sales pitch and end with a hands-on self-guided workshop. The the instructions for the workshops are at <https://github.com/PaloAltoNetworks/prisma-cloud-bootcamps> which I forked into a personal repo at <https://github.com/MasonDenney/prisma-cloud-bootcamps>.

## Bridgecrew
Several years ago Palo Alto Networks acquired Bridgecrew. Bridgecrew created checkov as well as a few other tools. At the end of the workshop instructions you can find a list of vulnerable by design terraform repos by bridegecrew such as:
- [TerraGoat - Vulnerable by design Terraform Infrastructure](https://github.com/bridgecrewio/terragoat)
- [Cfngoat - Vulnerable by design Cloudformation Template](https://github.com/bridgecrewio/cfngoat)
- [CdkGoat - Vulnerable by design AWS CDK Infrastructure](https://github.com/bridgecrewio/cdkgoat)
- [BicepGoat - Vulnerable by design Bicep and ARM Infrastructure](https://github.com/bridgecrewio/bicepgoat)
- [KubernetesGoat-Vulnerable by design KubernetesCluster](https://github.com/bridgecrewio/kubernetes-goattest)
- [KustomizeGoat - Vulnerable by design Kustomize deployment](https://github.com/bridgecrewio/kustomizegoat)
- [SupplyGoat- Vulnerable by design SCA](https://github.com/bridgecrewio/supplygoat)

## Prisma Cloud Terraform Providers
You can find all PaloAltoNetworks Providers here: <https://registry.terraform.io/search/providers?q=PaloAltoNetworks>
- prismacloud = <https://registry.terraform.io/providers/PaloAltoNetworks/prismacloud/latest>
- prismacloudcompute = <https://registry.terraform.io/providers/PaloAltoNetworks/prismacloudcompute/latest>
- bridgecrew = <https://registry.terraform.io/providers/PaloAltoNetworks/bridgecrew/latest>

## Prisma Cloud APIs
There are different APIs that have different URLs and schemas, enterprise docs at <https://pan.dev/prisma-cloud/api/>
- CSPM = <https://pan.dev/prisma-cloud/api/cspm/>
- CWPP = <https://pan.dev/prisma-cloud/api/cwpp/>
- CAS = <https://pan.dev/prisma-cloud/api/code/>