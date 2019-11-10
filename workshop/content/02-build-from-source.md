## Source-to-Image

Source-to-Image combines source repos and operationally-maintained builder images to produce application images. It's included with OpenShift and is also available as a standalone project, for use with Jenkins or other external builder processes: [Source-to-Image](https://github.com/openshift/source-to-image)

## Deploy from the Web Console

There are multiple ways to deploy applications. We'll start by deploying a simple application via the Developer Perspective in the web console. In the [web console](%console_url%) toggle back to the Developer Perspective if necessary.

Click **From Git** on the **Topology** page or click the **+Add** item in the left navigation and then choose **From Git**. Enter the following Git URL:

```copy
https://github.com/ryanj/http-base
```

Select **Node.js** and leave version 10 selected by default, then click **Create**.

You'll be directed back to the **Topology** page, where you can see the status of the deployment. If you click the build status icon in the bottom left corner of the `http-ex` deployment, you'll be able to view the build logs. Click **Topology** in the left navigation to return to the Topology page.

Once the build is complete and the circle around the `http-ex` component turns blue, click the Open URL icon in the top right corner of the `http-ex` deployment to open the URL in your browser. 

## Deploy from the Command Line

Next, we'll deploy a sample app from the command line using `oc`. If you have a GitHub account, sign in and fork this repository:

[Beer Name Demo](https://github.com/jankleinert/redis-session-demo)

If you don't have a GitHub account, or don't want to create a fork, you can just clone that repo. However, you won't be able to set up Webhooks in a later step if you choose not to create a fork. 

Switch back to the [Terminal](%terminal_url%).

Edit the command below as necessary to point to either your fork or the original repository:

```copy-and-edit
git clone https://github.com/jankleinert/redis-session-demo
```

Then continue with the following:
```execute-1
cd redis-session-demo
git checkout beer-demo
```

Now that you have the source code, we'll use `oc new-app` to build the image and deploy the application.

```execute-1
oc new-app . --name beer-demo --env PORT=8080
```

That command is telling OpenShift that we want to create an application based on the source code in the current git repository (with a public remote). The `name` flag gives a name to the resources generated, and the `env` flag sets an environment variable. Our application uses port 3000 by default, but we want to use 8080 instead, so we are setting that with the environment variable.

When you create an application using `oc new-app` it doesn't not create a Route (publicly accessible URL) automatically. To expose the service and create a Route, use this command:

```execute-1
oc expose svc/beer-demo
```

You can view the status via the command line:
```execute-1
oc status
```

Alternatively, you can go back to the web console and view it from there. Once the application has finished building and deploying, you can access it in your browser.

Either get the URL by running:
```execute-1
oc get routes
```
or by clicking the Open URL icon for `beer-demo` on the Topology page or [this link](http://beer-demo-%project_namespace%.%cluster_submdomain%).



Now you've deployed applications two ways: with the web console and with the command line. Next you'll learn how to set up git webhooks so that any new changes to your code trigger a new build and deployment. 

