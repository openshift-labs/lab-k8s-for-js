Lab - Hands On Intro to Kubernetes and OpenShift for JS Developers
====================

This workshop provides an introduction to Kubernetes and OpenShift for JS Developers.

The workshop uses the HomeRoom workshop environment in the learning portal configuration.
You will need to be a cluster admin in order to deploy it.

When the URL for the workshop environment is accessed, a workshop session will be created on demand.

Deploying the Workshop
----------------------

To deploy the workshop, first clone this Git repository to your own machine.

When you clone the repository, ensure you use the `--recurse-submodules` option with the `git clone` command. Alternatively, after having cloned the repository, within the repository directory, run:

```
git submodule update --init --recursive
```

The option to `git clone`, or the `git submodule update` command, ensure that a copy of a Git submodule which contains scripts to help you deploy the workshop will also be cloned.

Next create a project in OpenShift into which the workshop is to be deployed.

```
oc new-project workshops
```

From within the top level of the Git repository, now run the command below.

```
.workshop/scripts/deploy-spawner.sh
```

**NOTE:** The default max number of sessions (i.e. users who can access the workshop at the same time) defaults to 50. The number for the max number
of sessions can be overridden when running the `deploy-spawner.sh` script by supplying the `--override SERVER_LIMIT=50` option. If 50 is not enough sessions, or too many, the amount can be increased or decreased by changing the value from 50 to the expected number of users. Your cluster should have enough resources to support the number of users in your workshop. Each user is given a quota of 8Gi of memory. If you don't set this up front and need to change it later, you can edit the `SERVER_LIMIT` environment variable on the spawner deployment config. Changing it will cause all currently running sessions to be stopped, so avoid changing it during a workshop.

When deployed the name of the deployment will be `lab-k8s-for-js`.

You can determine the hostname for the URL to access the workshop by running:

```
oc get route lab-k8s-for-js
```

When the URL is accessed, you will be prompted to login. For the user name, use your email address or other name which would uniquely identify you. This is just used as an identifier for your session if you need to login again to the same session again. What you use for the user name isn't recorded in any way. For the password, use "openshift".

Editing the Workshop
--------------------

The deployment created above will use a version of the workshop which has been pre-built into an image and which is hosted on `quay.io`.

To make changes to the workshop content and test them, edit the files in the Git repository and then run:

```
.workshop/scripts/build-workshop.sh
```

This will replace the existing image used by the active deployment.

If you are running an existing instance of the workshop, from your web browser select "Restart Workshop" from the menu top right of the workshop environment dashboard.

When you are happy with your changes, push them back to the remote Git repository. This will automatically trigger a new build of the image hosted on `quay.io`.

If you need to change the RBAC definitions, or what resources are created when a project is created, change the definitions in the `templates` directory. You can then re-run:

```
.workshop/scripts/deploy-spawner.sh
```

and it will update the active definitions.

Note that if you do this, you will need to re-run:

```
.workshop/scripts/build-workshop.sh
```

to have any local content changes be used once again as it will revert back to using the image on ``quay.io``.

If you need to ever update the deployment scripts pulled in via a git submodule to the latest version, run:

```
git submodule update --recursive --remote
```

The update will be staged immediately, so don't forget to commit it.

Deleting the Workshop
---------------------

To delete the spawner and any active sessions, including projects, run:

```
.workshop/scripts/delete-spawner.sh
```

To delete the build configuration for the workshop image, run:

```
.workshop/scripts/delete-workshop.sh
```

To delete special resources for CRDs and cluster roles for the Tekton operator, run:

```
.workshop/scripts/delete-resources.sh
```

Ideally this workshop environment should only be deployed in an expendable cluster, and not one which is shared for other work.
