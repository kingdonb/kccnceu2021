# Helm Users! What Can Flux v2 Do For You

author
:   @kingdonb and @r6by

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
* Demo After Slides

# DEMO

Blog on Kubernetes

**Live Activity**

* `flux bootstrap`
* Kyverno
* Webhook Receiver
* Wordpress

# DEMO

(just kidding)

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
* (image-automation-controller)
* (image-reflector-controller)

# kyverno

Requirement to make Flux secure for multi-tenancy

(Keep Kubernetes safe from Wordpress)

# webhook

Receiver configuration allows Flux to sync instantly on `git push`

# Wordpress

Another application installed via Helm

# Jenkins

Another application installed via Helm

# Terminal

Another application installed via Helm

# Linkerd2

Not just another application

(Another application installed via Helm)

# Flagger

Progressive Delivery with Flux and Flagger

Another project in the FluxCD umbrella

# So Much More

We wanted to show you so much more

* Mozilla SOPS

Secrets Management

# So Much More

We wanted to show you so much more

* `alexellis/arkade` - opinionated Helm (and other) packages for your Kubernetes cluster

Package Management

# So Much More

We wanted to show you so much more

* image-automation-controller
* image-reflector-controller

Flux v2's new and improved image automation

# So Much More

We wanted to show you so much more

* cert-manager

Let's Encrypt certificates

# So Much More

We wanted to show you so much more

* [loft-sh/kiosk](https://github.com/loft-sh/kiosk)
* [loft.sh](https://loft.sh)

Manage multi-tenancy (entry-level configuration)

# So Much More

We wanted to show you so much more

* ingress configuration

(We used nginx-ingress)

# So Much More

We wanted to show you so much more

* [Rabbit](https://rabbit-shocker.org)

A presentation tool for Rubyist

(How we got those neat tortoise/hare icons at the bottom of the screen!)

# DEMO 2

Bootstrapping with Kind (Kubernetes in Docker)

`https://gist.github.com/scottrigby/65c1fbb1f6437f6cfa11b594cf8e26a9`

# Thanks

Thank You (Audience)

Thanks to KubeCon and CloudNativeCon 2021 Organizers

Thanks to CNCF
