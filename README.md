[![Docker Repository on Quay](https://quay.io/repository/citrix/citrix-k8s-ingress-controller/status "Docker Repository on Quay")](https://quay.io/repository/citrix/citrix-k8s-ingress-controller)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](./license/LICENSE)
[![GitHub stars](https://img.shields.io/github/stars/citrix/citrix-k8s-ingress-controller.svg)](https://github.com/citrix/citrix-k8s-ingress-controller/stargazers)
[![HitCount](http://hits.dwyl.com/citrix/citrix-k8s-ingress-controller.svg)](http://hits.dwyl.com/citrix/citrix-k8s-ingress-controller)
[![Artifact Hub](https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/netscaler)](https://artifacthub.io/packages/search?repo=netscaler)
---

# Citrix ingress controller

## Description

This repository contains the Citrix ingress controller built around [Kubernetes Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/).
##### Participate:

   You can discuss questions/bugs/feature requests for this project on our Slack channel. To request an invitation to participate in the [Citrix ADC Cloud Native Slack channel](https://citrixadccloudnative.slack.com), provide your email address using the following form:
   https://podio.com/webforms/22979270/1633242. 
   You can also report issues using the bug reporting template.

## What is an ingress controller?

An Ingress Controller is a [controller](https://kubernetes.io/docs/concepts/architecture/controller/) that watches the Kubernetes API server for updates to the Ingress resource and reconfigures the Ingress load balancer accordingly.

## What is the Citrix ingress controller?

Citrix provides an ingress controller for Citrix ADC MPX (hardware), Citrix ADC VPX (virtualized), and Citrix ADC CPX (containerized) for [bare metal](https://github.com/citrix/citrix-k8s-ingress-controller/tree/master/deployment/baremetal) and [cloud](https://github.com/citrix/citrix-k8s-ingress-controller/tree/master/deployment) deployments. It is built around Kubernetes [Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/) and automatically configures Citrix ADC based on the Ingress resource configuration.

The Citrix ingress controller can be deployed either by directly using [yamls](https://github.com/citrix/citrix-k8s-ingress-controller/tree/master/deployment/baremetal) or by [helm charts](https://github.com/citrix/citrix-k8s-ingress-controller/tree/master/charts).

## Features

Features supported by the Citrix ingress controller can be found [here](https://github.com/citrix/citrix-k8s-ingress-controller/tree/master/deployment).
The Citrix API Gateway features can be found [here](https://github.com/citrix/citrix-k8s-ingress-controller/blob/master/docs/deploy/citrix-api-gateway.md).

## Supported platforms and deployments

Click [here](docs/support-matrix.md) for detailed list of supported platforms and deployments.

## Documentation

For detailed documentation, see the [Citrix ingress controller Live Documentation](https://developer-docs.citrix.com/projects/citrix-k8s-ingress-controller/en/latest/).

## Deployment solutions

You can deploy the Citrix ingress controller in many platforms. For detailed information, see [Deployment Architecture](https://github.com/citrix/citrix-k8s-ingress-controller/tree/master/deployment).

## Examples

Deploy the Guestbook application and use the [Citrix ADC CPX](https://www.citrix.com/products/citrix-adc/cpx-express.html) to provide the Ingress:

-  [Quick Deploy using YAML](./example)
-  [Quick Deploy using Helm](https://github.com/citrix/citrix-helm-charts/tree/master/examples/citrix-cpx-with-ingress-controller)
-  [Quick Deploy using Kops](./docs/deploy/deploy-cic-kops.md)
-  [Deployment in Google Cloud](https://github.com/citrix/citrix-k8s-ingress-controller/blob/master/deployment/gcp)
-  [Deployment in Azure Cloud](https://github.com/citrix/citrix-k8s-ingress-controller/tree/master/deployment/azure)

## Release notes

Click [here](https://github.com/citrix/citrix-k8s-ingress-controller/releases) for the release notes of the latest Citrix ingress controller release.

## Questions and support

For questions and support the following channels are available:

-  [Citrix Discussion Forum](https://discussions.citrix.com/forum/1657-netscaler-cpx/)
-  [Citrix ADC Cloud Native Slack Channel](https://citrixadccloudnative.slack.com/)

To request an invitation to participate in the Slack channel, provide your email address using this form: [https://podio.com/webforms/22979270/1633242](https://podio.com/webforms/22979270/1633242)

For more information about Citrix cloud native solutions, you can reach out to the Citrix product team at: AppModernization@citrix.com

![ ](./docs/media/contact-product-team.png)

## Issues

Describe issues in detail, collect logs, and use the [discussion forum](https://discussions.citrix.com/forum/1657-netscaler-cpx/) to report issues.

Use the following command to collect logs:

```
Get Logs: kubectl logs citrix-k8s-ingress-controller > log_file
```

You can report any issues using the following forum:
`https://discussions.citrix.com/forum/1657-netscaler-cpx/`

For information on how to troubleshoot some of the common issues that you may encounter while using Citrix ADC CPX, see the
[Citrix ADC CPX documentation](https://docs.citrix.com/en-us/citrix-adc-cpx/current-release/cpx-troubleshooting.html).

## Code of Conduct

This project adheres to the [Kubernetes Community Code of Conduct](https://github.com/kubernetes/community/blob/master/code-of-conduct.md). By participating in this project you agree to abide by its terms.

## License

[Apache License 2.0](./license/LICENSE)
