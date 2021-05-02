# Helm Users! What Flux 2 Can Do For You

Helm, the Package manager for Kubernetes. Flux, the GitOps continuous delivery solution for Kubernetes. Both can be used independently, but are more powerful together.

Scott Rigby, Helm and Flux maintainer - and Kingdon Barrett, OSS engineer - will share the benefits of Helm and GitOps for developers, with live demos showcasing the extra awesomeness of Flux v2 and Helm together. This talk is for Helm users who have either never used Flux, or Flux v1 users looking forward to new features in Flux v2.

[sched.co/iE1e](https://sched.co/iE1e)

Wednesday, May 5 - 13:10 - 13:45 CEST (Central European Summer Time)

Wednesday, May 5 -  7:10 -  7:45 EDT  (Eastern US Daylight Savings Time)

## For attendee

Welcome to our talk at KubeCon EU!

This repository lives at [github.com/kingdonb/kccnceu2021](https://github.com/kingdonb/kccnceu2021)

You can repeat the examples presented in our talk on any Kubernetes cluster; start by forking this repository. Then, install the latest version of the Flux CLI binary per the instructions in the [Flux Getting Started Guide][install-the-flux-cli].

### Prerequisites

Get any Kubernetes cluster (v1.16 or greater) ready to use in your `KUBECONFIG` and run `flux check --pre` to verify that everything is ready. Your output should look like this:

```
► checking prerequisites
✔ kubectl 1.21.0 >=1.18.0-0
✔ Kubernetes 1.20.2 >=1.16.0-0
✔ prerequisites checks passed
```

Assuming that looks OK, add your GitHub User ID in a variable as follows, and run `flux bootstrap`:

```
GITHUB_USER=kingdonb

flux bootstrap github --path=./clusters/my-cluster/ \
  --personal --owner=$GITHUB_USER --repository=kccnceu2021 --branch=main \
  --components-extra=image-reflector-controller,image-automation-controller
```

The output should look like this:

```
✗ GITHUB_TOKEN environment variable not found
```

Set and export the `GITHUB_TOKEN` variable as described in the [Getting Started Guide][prerequisites], section **Prerequisites**. (This guide mentions support for use with GitLab, which can also be used instead of GitHub.)

A personal access token is needed so that Flux can set up read-only deploy keys. Pass the `--token-auth` if you want to give Flux read-write access, (this is only used for the Image Automation part of the demonstration.)

Your output should look something like this:

```
► connecting to github.com
► cloning branch "main" from Git repository "https://github.com/kingdonb/kccnceu2021.git"
✔ cloned repository
► generating component manifests
✔ generated component manifests
✔ committed sync manifests to "main" ("6440f0d9fbf9b6df29f1f18ce5d4bda980afe5aa")
► pushing component manifests to "https://github.com/kingdonb/kccnceu2021.git"
► installing toolkit.fluxcd.io CRDs
◎ waiting for CRDs to be reconciled
✔ CRDs reconciled successfully
► installing components in "flux-system" namespace
✔ installed components
✔ reconciled components
► determining if source secret "flux-system/flux-system" exists
► generating source secret
✔ public key: ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDeAB50bhTnDYRCO+aa7bnuC72Nm+hiLrx6kPMdQvN4WzuF09xwLXCiXgFO/VXpyraZzvlIMqrWZxik8VHVFheRd7sirDAToFB4mj2T9ZIRz9r/ioQf9BdKo0wYah6oYS7q+RuzQuC1GQeFqkZl3wrx/75uQ13TS1wxddMzCjFhVOj2pIyWBv+wfbjMK+v6OtnG4tKcd00FZhZvI7KPViile2HqRqFY03ShBjJ8obid90DcYoov/XmnnyWzkqkX/U6AijC4kG1f9k3PA7S0rTo+fLpYtXODWrHNt+dQ7ieu3Xj/f66igTtrwhS1jzjR79QpGLlU9vETU2X7ww1Qv713
✔ configured deploy key "flux-system-main-flux-system-./clusters/my-cluster" for "https://github.com/kingdonb/kccnceu2021"
► applying source secret "flux-system/flux-system"
✔ reconciled source secret
► generating sync manifests
✔ generated sync manifests
✔ sync manifests are up to date
► applying sync manifests
✔ reconciled sync configuration
◎ waiting for Kustomization "flux-system/flux-system" to be reconciled
✔ Kustomization reconciled successfully
► confirming components are healthy
✔ helm-controller: deployment ready
✔ notification-controller: deployment ready
✔ image-reflector-controller: deployment ready
✔ image-automation-controller: deployment ready
✔ source-controller: deployment ready
✔ kustomize-controller: deployment ready
✔ all components are healthy
```

Now, run the post-installation check:

```
$ flux check
► checking prerequisites
✔ kubectl 1.21.0 >=1.18.0-0
✔ Kubernetes 1.20.2 >=1.16.0-0
► checking controllers
✔ helm-controller: deployment ready
► ghcr.io/fluxcd/helm-controller:v0.10.0
✔ image-automation-controller: deployment ready
► ghcr.io/fluxcd/image-automation-controller:v0.9.0
✔ image-reflector-controller: deployment ready
► ghcr.io/fluxcd/image-reflector-controller:v0.9.1
✔ kustomize-controller: deployment ready
► ghcr.io/fluxcd/kustomize-controller:v0.12.0
✔ notification-controller: deployment ready
► ghcr.io/fluxcd/notification-controller:v0.13.0
✔ source-controller: deployment ready
► ghcr.io/fluxcd/source-controller:v0.12.1
✔ all checks passed
```

If your output looks as above, great! Check on the `git` `source` statuses next:

```
$ flux get sources git
NAME       	READY	MESSAGE                                                        	REVISION                                     	SUSPENDED
flagger    	True 	Fetched revision: main/c9257bdb99410d5cafd3ae7f18fc0616b90f2c51	main/c9257bdb99410d5cafd3ae7f18fc0616b90f2c51	False
flux-system	True 	Fetched revision: main/acb14c363287833bcbc2c38a09aff4e37148cead	main/acb14c363287833bcbc2c38a09aff4e37148cead	False
```

We've synchronized the flagger repo ([github.com/fluxcd/flagger][flagger])

and the flux-system repo (this is your fork of [kingdonb/kccnceu2021][kccnceu2021]) – double great!

Assuming that was all in order, let's check on the Kustomize controller which is reconciling `flux-system`:

```
$ flux get kustomizations
NAME       	READY	MESSAGE                                                                                                                                                          	REVISION                                     	SUSPENDED
flagger    	False	apply failed: Warning: rbac.authorization.k8s.io/v1beta1 ClusterRole is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 ClusterRole	                                             	False
           	     	Warning: rbac.authorization.k8s.io/v1beta1 ClusterRoleBinding is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 ClusterRoleBinding
           	     	Error from server (NotFound): error when creating "fceed576-ba9c-4a56-974c-7988cbf737c1.yaml": namespaces "linkerd" not found
           	     	Error from server (NotFound): error when creating "fceed576-ba9c-4a56-974c-7988cbf737c1.yaml": namespaces "linkerd" not found

flux-system	True 	Applied revision: main/acb14c363287833bcbc2c38a09aff4e37148cead                                                                                                  	main/acb14c363287833bcbc2c38a09aff4e37148cead	False
kyverno-hr 	True 	Applied revision: main/acb14c363287833bcbc2c38a09aff4e37148cead                                                                                                  	main/acb14c363287833bcbc2c38a09aff4e37148cead	False
my-secrets 	False	decryption secret error: Secret "sops-gpg" not found                                                                                                             	                                             	False
```

Uh-oh! This looks like something is wrong. Time to get to work... the error message tells us that the Flagger kustomization I tried to get you to install, depends on linkerd2. Why haven't we installed linkerd for you?

### Install Linkerd

We're actually using this opportunity to point out `arkade` which you can use to get Linkerd2 installed. Find it here, [alexellis/arkade](https://github.com/alexellis/arkade) – download the arkade CLI with the following command:

```
curl -sLS https://dl.get-arkade.dev | sh
```

Or, you can feel free to make that a `sudo sh` if you like to live dangerously. 😉

```
arkade install linkerd
```

(... the output is long and noisy, but should result in a bunch of checkmarks and finally wind up like this!)

This is not a presentation about Linkerd, in fact we're using Arkade as our excuse to avoid talking about Linkerd, the theme of this presentation is Helm and Flux – and although Linkerd provides a Helm chart, it is more cumbersome to use than their CLI, which is easy to use, and made even easier by `arkade`.

```
Status check results are √
=======================================================================
= Linkerd has been installed.                                        =
=======================================================================

# Find out more at:
# https://linkerd.io

# To use the linkerd2 CLI set this path:

export PATH=$PATH:/Users/kingdonb/.arkade/bin
linkerd2 --help

Thanks for using arkade!
```

Now why were we installing linkerd? Oh yes, Flagger's kustomization was spamming errors demanding it! Now that we've taken care of this, we should see Flagger deployed successfully as below:

```
$ flux get ks
NAME       	READY	MESSAGE                                                        	REVISION                                     	SUSPENDED
flagger    	True 	Applied revision: main/c9257bdb99410d5cafd3ae7f18fc0616b90f2c51	main/c9257bdb99410d5cafd3ae7f18fc0616b90f2c51	False
flux-system	True 	Applied revision: main/acb14c363287833bcbc2c38a09aff4e37148cead	main/acb14c363287833bcbc2c38a09aff4e37148cead	False
kyverno-hr 	True 	Applied revision: main/acb14c363287833bcbc2c38a09aff4e37148cead	main/acb14c363287833bcbc2c38a09aff4e37148cead	False
my-secrets 	False	decryption secret error: Secret "sops-gpg" not found           	                                             	False
```

It looks like Flagger is ready!

Go ahead and follow the [Flagger Bootstrap], if you want to learn more about Progressive Delivery! You've already completed the prerequisites, and used Flux to install Flagger.

That's not part of this demo, but we can come back to this later (come join us at the booth for impromptu Flagger demos! Ask Kingdon to show how it works.)

The above instructions put your cluster approximately equal with the starting point of our KubeCon 2021 EU presentation.

### kyverno

Continuing our discussion of "we actually need more than just a blog to have a blog", we want to show how we can keep the rest of our cluster safe from Wordpress, and if there are vulnerabilities in one application, we want to follow the principle of least privilege, limiting the risk of compromise to only resources that those applications would have needed to be able to access.

We'll need to set up some RBAC policies (later), but there's something important we need to address first: the `flux-system` namespace has some God Mode cluster role bindings that it uses to manage namespaces and cluster-wide resources as well as its own Custom Resource Definitions, and with secrets that potentially carry write access to our repository, (which we must protect as it is the single source of truth in our cluster!)

We need to do some work in order to be sure that tenants can't gain access to these sensitive resources.

Slide 06 links to the [fluxcd/flux2-multi-tenancy] guide which explains: *To enforce tenant isolation, cluster admins must configure Flux to reconcile the Kustomization and HelmRelease kinds by impersonating a service account from the namespace where these objects are created. In order to make the spec.ServiceAccountName field mandatory, you should use a validation webhook, for example Kyverno or OPA Gatekeeper. On cluster bootstrap, you need to configure Flux to deploy the validation webhook and its policies before reconciling the tenants repositories.*

This can be configured by adding the `kyverno-policies/flux-multi-tenancy.yaml` to our cluster's YAML manifests, and checking that it is enforced properly. (Find it [here][kyverno-policies].) This is considered the bare minimum policy config for Flux v2 in multi-tenant mode, at least until [flux2#582][https://github.com/fluxcd/flux2/pull/582] is adopted and implemented.

In the `main` branch, and in our talk, we guessed and fumbled a little about how to install Kyverno with Helm Controller. Don't make the same mistake, check the [fluxcd/flux2-multi-tenancy] tutorial guide and follow it carefully instead. Helm is not good with CRDs. Find the solution in the `present` branch, where we've applied the pattern shown in the tutorial to ensure that kyverno-policies are enforced before tenant workloads start to be configured.

See if you can come up with the solution with the help of the tutorial, or skip to find the solution in the `present` branch:

`git checkout present`

For the rest of this guide, the solutions have already been prepared to work appropriately in multi-tenant mode on the repo's `present` branch. Some changes are needed with the multi-tenant config enabled, (which differs slightly from what is shown in our talk.)

We cannot put everything in the root `flux-system` kustomization anymore in our new multi-tenant environment.

As each example that follows is documented here, we will move our tenant workloads into the `tenants` directory and infrastructure into the `infrastructure` directory when sync order issues get in our way. Wordpress and other potentially troublesome tenants go in the `tenants` directory, where the Kyverno policy is guaranteed.

This also importantly gets us a way to start moving things out of `flux-system`, which runs before Kyverno as well, and so it can also get away with some things that Kyverno would otherwise definitely prevent from entering into the cluster with its admission webhook.

If this worked, new manifests in the `flux-system` sync will also be validated by the `flux-multi-tenant` policies that we have added. (Let's check our existing HelmReleases probably all are missing `serviceAccountName`, delete one of our helm releases and let the `flux-system` Kustomization try to apply it again, just to be sure it will be blocked like we expected.)

```
$ flux get kustomizations
...

Resource: "helm.toolkit.fluxcd.io/v2beta1, Resource=helmreleases", GroupVersionKind: "helm.toolkit.fluxcd.io/v2beta1, Kind=HelmRelease"
    Name: "terminal", Namespace: "terminal"
    for: "3736b27f-0b38-40e0-ab45-5fd22e83020c.yaml": admission webhook "validate.kyverno.svc" denied the request:
    resource HelmRelease/terminal/terminal was blocked due to the following policies
    flux-multi-tenancy:
      serviceAccountName: 'validation error: .spec.serviceAccountName is required. Rule serviceAccountName failed at path /spec/serviceAccountName/'
```

Perfect! That's exactly what the multi-tenancy guide said we needed Kyverno to prevent. (Explaining the details of [fluxcd/flux2-multi-tenancy] is out of scope for this tutorial, but you should follow it separately if you want to understand Flux's multi-tenant configuration more thoroughly.)

### webhook

We've created a Receiver, and if you're following along you'd probably like to enable it now, as with each new flux Kustomization resource we're adding, we're getting tired of running `flux reconcile` when a Git webhook receiver could be doing it for us.

Similarly you'll want a notification Alert Provider such as is demonstrated in the [Notifications Guide](https://fluxcd.io/docs/guides/notifications/), which will save you from typing `flux get kustomizations` when you need to find out the status of your pushed deployment.

You can tune in to the channel where notifications are broadcasted, on whatever chat or teams service.

The webhook Receiver lets Flux know whenever a git commit is pushed.

If you noticed an error related to `sops-gpg` earlier in our demo, it is in fact related to the reason why our webhook is not already working right this very moment!

The [Webhook Receiver Guide](https://fluxcd.io/docs/guides/webhook-receivers/) explains, you need to generate a random string and create a secret with a `token` field. We've already done that, but then we encrypted it so you can't retrieve it, because you don't have our cluster keys.

Go ahead and follow the guide, or skip to the step where you generate a secret and populate another one. It's OK to skip encrypted secrets for now, if you're just here for the Helm talk, we won't get offended!

A reference to the SOPS guide is placed later on, so you can encrypt secrets and store them in the repository too, just like we did in the demo.

Follow the SOPS guide to generate your own `sops-gpg` secret, and overwrite the file `secrets/my-cluster/webhook-token-secret.yaml` with one that you generated.

The steps to generate the webhook token secret are in the Webhook Receiver guide. The steps to encrypt it properly are in the Mozilla SOPS guide. Read both guides accordingly to gain a thorough understanding of both topics!

The short version, once you have generated your GPG key from the guide and set up `.sops.yaml` in the cluster directory and in the secrets directory, is this:

```
TOKEN=$(head -c 12 /dev/urandom | shasum | cut -d ' ' -f1)
echo $TOKEN

kubectl -n flux-system create secret generic webhook-token \
--from-literal=token=$TOKEN --dry-run=client -oyaml > secrets/my-cluster/webhook-token-secret.yaml

cd secrets/my-cluster/
sops -e -i webhook-token-secret.yaml
```

Until the `webhook-token` secret is ready, the Webhook Receiver will continue reporting this error condition:

```
$ flux get receivers
NAME       	READY	MESSAGE                                                                                             	SUSPENDED
flux-system	False	unable to read token from secret 'flux-system/webhook-token' error: Secret "webhook-token" not found	False
```

With this change, if we tell Flux to reconcile the receiver, (or if we just wait a little longer) it looks somewhat better now!

```
$ flux reconcile receiver flux-system
► annotating Receiver flux-system in flux-system namespace
✔ Receiver annotated
◎ waiting for Receiver reconciliation
✔ Receiver reconciliation completed
$ flux get receivers
NAME       	READY	MESSAGE                                                                                              	SUSPENDED
flux-system	True 	Receiver initialised with URL: /hook/69660bb1b387b0962f56fd958ae2570262c6cb55cd6c5d787f3851dc0de4ff61	False
```

It looks much better now! Visit the webhook configuration page `https://github.com/[YOUR_GITHUB_ID]/kccnceu2021/settings/hooks` and finish the configuration there, following the steps described in the guide.

We can test our webhook by pushing another commit and watching the output of `kubectl get ks -w -n flux-system` and `kubectl get gitrepositories -w -n flux-system` for an idea of how quickly the effect occurs.

#### It didn't work?

It didn't work! On our cluster, we tried using a Load Balancer for the webhook receiver, but it couldn't communicate in `flux-system` due to network policy.

I'm afraid this is a bug, or at least gap in our documentation. The webhook receiver guide only explains how to expose the webhook receiver service with a load balancer. The network policy (at least on DigitalOcean's managed K8s CNI, Cilium) prevented communication between the load balancer and the cluster IP service.

I switched to `Ingress` for the receiver instead, which is also demonstrated in the **present** branch of the `kingdonb/kccnceu2021` repo.

Moving on... (it works!)

### Wordpress

Go to the `wordpress` namespace

```
kubectl config set-context --current --namespace=wordpress
```

Find Wordpress is running here!

```
kubectl get all
```

[bitnami/charts ~ wordpress](https://github.com/bitnami/charts/tree/master/bitnami/wordpress/#installing-the-chart)

### Jenkins

Go to the `jenkins` namespace

```
kubens jenkins
```

Find Jenkins is running here!

[jenkinsci/helm-charts](https://github.com/jenkinsci/helm-charts)

### Terminal

[GoTTY](https://github.com/okteto/charts/tree/master/terminal)

### Linkerd2

[Linkerd ~ Getting Started](https://linkerd.io/2.10/getting-started/)

### Flagger

[GitOps Hands-on](https://eks.handson.flagger.dev/canary/#application-bootstrap)

### Flux2 Kustomize Helm

[flux2-kustomize-helm-example](https://github.com/fluxcd/flux2-kustomize-helm-example)

### Mozilla SOPS

[Mozilla SOPS Guide](https://fluxcd.io/docs/guides/mozilla-sops/)

### Automate image updates

[Image Update Guide](https://fluxcd.io/docs/guides/image-update/)

### arkade

[alexellis/arkade](https://github.com/alexellis/arkade)

### cert-manager

[cert-manager.io ~ configuring-your-first-issuer](https://cert-manager.io/docs/installation/kubernetes/#configuring-your-first-issuer)

### kiosk

[loft-sh/kiosk ~ why-kiosk](https://github.com/loft-sh/kiosk#why-kiosk)

### ingress-nginx

[kubernetes.io ~ ingress-nginx](https://kubernetes.github.io/ingress-nginx/deploy/#using-helm)

### Rabbit

[rabbit-shocker.org ~ en](https://rabbit-shocker.org/en/)

### bitly

There is another walkthrough of this presentation at
[bit.ly/32rUInK](https://bit.ly/32rUInK)

Find the gist there!


## Thanks

Thank You for following
@yebyen - Kingdon Barrett
@r6by - Scott Rigby

Developer Experience, Weaveworks

### Flux Pavilion!

Visit us in person at the Flux Pavilion at KC/CNC EU 2021
!!!


## For author

### Show

    rake

### Publish

    rake publish

## For viewers

### Install

    gem install rabbit-slide-yebyen-kccnceu2021

### Show

    rabbit rabbit-slide-yebyen-kccnceu2021.gem

[install-the-flux-cli]: https://fluxcd.io/docs/get-started/#install-the-flux-cli
[prerequisites]: https://fluxcd.io/docs/get-started/#prerequisites
[flagger]: https://github.com/fluxcd/flagger
[kccnceu2021]: https://github.com/kingdonb/kccnceu2021
[Flagger Bootstrap]: https://docs.flagger.app/tutorials/linkerd-progressive-delivery#bootstrap
[kyverno-policies]: https://github.com/fluxcd/flux2-multi-tenancy/tree/main/infrastructure/kyverno-policies
