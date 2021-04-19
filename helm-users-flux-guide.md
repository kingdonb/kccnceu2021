# Helm Users! What Can Flux v2 Do For You

author
:   @yebyen and @r6by

date
:   2021-05-05

allotted-time
:   35m

start-time
:   2021-05-05T13:10:00+02:00

end-time
:   2021-05-05T13:45:00+02:00


# Outline

* Talk about Flux + Helm Controller
* Flux CRDs - HelmRelease, Kustomization, (...)
* Demo During Slides (**N**arrow **E**xpand)

# DEMO

Blog on Kubernetes

**Live Activity**

* `flux bootstrap`
* Kyverno
* Webhook Receiver
* Wordpress

# DEMO

(just kidding - more)

**Live Activity**

* Jenkins
* Okteto Terminal
* Linkerd2
* Flagger

# `flux bootstrap`

We will install all controllers

* source controller
* kustomize controller
* helm controller

# `flux bootstrap`

We will install all controllers

* notification controller
* (image-automation)
* (image-reflector)

# kyverno

Requirement to make Flux secure for multi-tenancy

(Keep Kubernetes safe from Wordpress)

[fluxcd/flux2-multi-tenancy](https://github.com/fluxcd/flux2-multi-tenancy#readme)

Follow this carefully!

# webhook

`Receiver` config allows Flux to sync instantly on `git push`

[flux-system-receiver.yaml](/kingdonb/kccnceu2021/tree/main/clusters/my-cluster/flux-resources/flux-system-receiver.yaml)
[webhook-lb.yaml](/kingdonb/kccnceu2021/tree/main/clusters/my-cluster/flux-resources/notification-controller-lb.yaml)

[webhook-receivers guide](https://toolkit.fluxcd.io/guides/webhook-receivers/)

# Wordpress

Another application installed via Helm

[bitnami/wordpress](https://github.com/bitnami/charts/tree/master/bitnami/wordpress)

# Jenkins

Another application installed via Helm

[jenkinsci/helm-charts](https://github.com/jenkinsci/helm-charts)

# Terminal

Another application installed via Helm

[okteto/terminal](https://github.com/okteto/terminal)

# Linkerd2

*Not just* another application

(Yes, Linkerd2 does provide Helm charts, but we don't install that way, we used `arkade` and `linkerd2` cli)

[linkerd getting started](https://linkerd.io/2.10/getting-started/)

[arkade install linkerd](https://github.com/openfaas/openfaas-linkerd-workshop#step-3-install-linkerd-2-onto-the-cluster)

# Flagger

Progressive Delivery with Flux and *Flagger*

Another project in the FluxCD umbrella

Depends on *Linkerd2* (option Istio, AWS App Mesh, nginx...)

# Flux2 Kustomize Helm

[fluxcd/flux2-kustomize-helm-example](https://github.com/fluxcd/flux2-kustomize-helm-example)

Example how to manage *multiple clusters*

(eg. staging, production)

# So Much More

We wanted to show you so much more

* Mozilla SOPS

Secrets Management

[mozilla-sops guide](https://toolkit.fluxcd.io/guides/mozilla-sops/)

# So Much More

We wanted to show you so much more

* image-automation
* image-reflector-controller

[Automate image updates to Git](https://toolkit.fluxcd.io/guides/image-update/)

# So Much More

We wanted to show you so much more

* *[arkade](https://github.com/alexellis/arkade)* - opinionated helm packages

Curated Constellations - make your own, or borrow opinions of others :) we show you ours

# So Much More

We wanted to show you so much more

* *cert-manager*

Let's Encrypt certificates

[cert-manager.io website](https://cert-manager.io)
[cert-manager.io/docs](https://cert-manager.io/docs/)

# So Much More

We wanted to show you so much more

* [loft-sh/kiosk](https://github.com/loft-sh/kiosk)
* [loft.sh](https://loft.sh)

Manage *multi-tenancy* another way

# So Much More

We wanted to show you so much more

* *ingress* configuration

(We used [nginx-ingress](https://kubernetes.github.io/ingress-nginx/deploy/#using-helm))

# So Much More

We wanted to show you so much more

* [Rabbit](https://rabbit-shocker.org)

A *presentation* tool for Rubyist

(neat tortoise/hare icons at the bottom of the slides!)

# DEMO NOTES

Follow along at home - gist following these examples

* [bit.ly/32rUInK](https://bit.ly/32rUInK)

`bit.ly/32rUInK`

*JIT presentation*! Instructions ready by the time you read this

# Thanks

Thank You (*Audience*) - Questions at the Flux Pavilion!

Thanks to CNCF, Linux Foundation, Weaveworks

Thanks to KubeCon and CloudNativeCon 2021 *Organizers*

# Questions

Visit *Flux Pavilion* - meet us live after - Kingdon and Scott, Weaveworks DX Team

After KCCNC EU 2021? _Slack_

Supported by *Volunteers* around the world

CNCF: `#flux` and `#flagger`
