**Important: Consider this as a POC - this is not production ready code and comes with no support.**

# sapui5-bas-cf-sample-app
Business Application Studio generated sample application to test, build and deploy to Cloud Foundry with the ui5 pipeline.

This application uses karma to run the tests. To do this a sidecar container running headless chrome is started, next to the node container.

For a description on how to setup a freshly generated BAS app please follow this guide: [enable-pipeline.-run.md](./enable_pipeline_run.md)

# Build pipeline mtaBuild and cfDeploy

The pipeline `azure-pipelines-marcus.yml` performs an mta build and a Cloud Foundry Deployment.

First the piper-go binary is looked up in a cache. In case there is no cache hit, the piper binary is downloaded via curl and put into the cache.

Afterwards we perform the mta build using docker image `devxci/mbtci`
After that we perform the cloud foundry deploy using docker image `ppiper/cf-cli`

# Multistage release
For demo purposes the release is using multiple stages: dev, qa, perftest and production which will be deployed to sequentially.

In Azure DevOps you can set up additional checks before for deploying to one of these environments.

# PR Validation
There's a separate definition of a pipeline which will be executed during a PR. This can be used to validate a PR before it will be accepted. As an example it contains the call of unit tests using npm test.

## Additional remarks:

Consider this as a POC - this is not production ready code and comes with no support.