## Background: Webhooks

Most Git repository servers support the concept of webhooks -- calling to an
external source via HTTP(S) when a change in the code repository happens.
OpenShift provides an API endpoint that supports receiving hooks from
remote systems in order to trigger builds. By pointing the code repository's
hook at the OpenShift API, automated code/build/deploy pipelines can be
achieved.

## Configuring GitHub Web Hooks
In this section you can use a build webhook to trigger a new build any time there is a change to your repository. In the the Developer perspective in the web console, click **Builds** in the left navigation. Click the `beer-demo` build config and scroll down to the Webhooks section.

There will be two entries in the Webhooks section. The GitHub webhook is the one we are interested in.

Click **Copy URL with Secret** for the GitHub webhook. Once you have the URL copied to your clipboard, navigate to your forked github repo.

Click the **Settings** link on the top right of the page.

Click on **Webhooks**, then the **Add Webhook** button

In the next screen, paste your link into the "Payload URL" field. You can leave the
secret token field blank -- the secret is already in the URL.

**NOTE**: If the URL you paste contains `kubernetes.default.svc`, it will need to be modified - contact one of the instructors.

Change the `Content Type` to `application/json`.

Finally, click on **Add Webhook**.

Now, each time you commit new source code to your GitHub
repository, a new build and deployment will occur inside of OpenShift.  Let's try
it out.

## Using GitHub Webhooks
In the beer-demo branch, open `/views/index.pug` either an editor or in the GitHub web interface and change the H1 tag from 

```
Click To Generate a Craft Beer Name!
```

to something else, such as 

```
Give Me a New Craft Beer Name!
```

Commit and push your changes.


Once you have done that, a **Build** should almost instantaneously be
triggered in OpenShift. From the **Topology** page, click the circle for `beer-demo` and look at the **Builds** section of the **Resources** tab, or, run the
following command to verify:

```execute-1
oc get builds
```

You should see that a new build is running:

```
NAME          TYPE      FROM          STATUS     STARTED          DURATION
beer-demo-1   Source    Git@b052ae6   Complete   17 minutes ago   1m27s
beer-demo-2   Source    Git@3b26e1a   Running    6 seconds ago
```

Once the build and deploy has finished, verify your new image was
automatically deployed by viewing the application in your browser:

[Craft Beer Name Demo]http://beer-demo-%project_namespace%.%cluster_submdomain%

You should now see the new H1 tag on the page.
