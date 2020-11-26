# sapui5-bas-cf-sample-app
Business Application Studio generated sample application to test, build and deploy to Cloud Foundry with the ui5 pipeline.

This application uses karma to run the tests. To do this a sidecar container running headless chrome is started, next to the node container.

For a description on how to setup a freshly generated BAS app please follow this guide: [enable-pipeline.-run.md](./enable_pipeline_run.md)

# Build pipeline mtaBuild and cfDeploy

The pipeline `azure-pipelines-marcus.yml` performs an mta build and a Cloud Foundry Deployment.

First the piper-go binary is looked up in a cache. In case there is no cache hit, the piper binary is downloaded via curl and put into the cache.

Afterwards we perform the mta build using docker image `devxci/mbtci`
After that we perform the cloud foundry deploy using docker image `ppiper/cf-cli`

## Additional remarks:

- Any docker image used in the azure context must be configured so that the default user is able to perform a user add. This it no true for our docker images. Our docker images relies on a user 1000 which must be configured in a proper way. Azure uses a user 1001 ouside and inside the containers. Before a container is launched they add the user 1001. Afterwards they launch the docker image with user 1001 and perform the task the image is designed for. User 1001 gets basically all file system permissions. We might come in trouble in case our images relies on some configurtation located in the $HOME directory of user 1000, since we are not running as that user (... again: our images are designed for being used with user 1000).

- piper binary cache handling needs to be improved. Currently the piper binary version is not part of the cache key. That will work fine in case we always use "latest and greatest". But in case we would like to distinguish versions we need to adjust.

- The user and password for cf deployment are provided in an encrypted was as pipeline parameters.

## Outlook

- The current pipeline is more the less for outlining the general way. It is a POC. In the long run we should provide tasks (e.g. mtaDeploy, cfDeploy). A user calls simply the task alongside with the required parameters. Things like e.g. piper binary handling (... downloading, caching), special handling wrt user 1001  should be transparent to the user in that case.
test