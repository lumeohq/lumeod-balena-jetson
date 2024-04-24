# ⚠️ **The Balena support was deprecated** ⚠️

Please check [in our documentation](https://docs.lumeo.com/docs/gateway) the guides to setup Lumeo Gateways for the configuration you desire - cloud, on-prem servers, or edge devices.

This repository is now read-only. 

---------------

# Lumeo Gateway Balena Image

Base app/image with lumeo gateway pre-installed. Experimental.
Currently only supports Jetson Xavier NX

Note: This approach requires you to maintain and push a new Balena release every time Lumeo updates Gateway docker containers. 

## Setup your Fleet
First, setup the following Fleet-wide or Device variables in Balena cloud:
```
LUMEO_APP_ID=<your Lumeo workspace app id>
LUMEO_API_KEY=<api key for your Lumeo workspace>
```
This will enable containers that you push to your fleet to automatically
register in this workspace.

## Prerequisite
Install balena-cli using the following instructions:

https://github.com/balena-io/balena-cli/blob/master/INSTALL.md

## Adjust Update Strategy
Balena update strategy is specified in docker-compose.yml with the `io.balena.update.strategy` label, defaults to "delete-then-download".
This will stop and delete the lumeo container, then download a new version. On devices with 16GB eMMC, this is required as the update image won't fit alongside the full running image. 
To reduce download bandwidth and downtime, remove the label if primary Balena storage is >32GB.

## Push new release
Push to the default device type of your fleet using :

```
$ balena push <fleet-name>. (i.e. balena push lumeo/lumeo-prod)
```

or

[![balena deploy button](https://www.balena.io/deploy.svg)](https://dashboard.balena-cloud.com/deploy?repoUrl=https://github.com/lumeohq/lumeod-balena-jetson&defaultDeviceType=jetson-xavier-nx-devkit)

Note: When using the button above, be sure to check the "Advanced" box and set the `LUMEO_APP_ID` and `LUMEO_API_KEY` placerholder env variables (or delete them from there if deploying to a fleet that already has those configured).

Once the application is running on your Balena device, it will auto register with Lumeo and appear in your Lumeo console.

## Setup Jetson Hardware
Finally, log into your hardware device (host, not the lumeo container) and set max perf mode : 

```.bash
$ balena ssh <device-uuid>

# NVP Mode 8 sets Jetson NX to 20W, 6 core (max perf).
$ nvpmodel -m 8
```
