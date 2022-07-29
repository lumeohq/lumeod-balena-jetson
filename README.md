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


## Push new release
Push to your fleet using :

```
$ balena push <fleet-name>
```

or 

[![balena deploy button](https://www.balena.io/deploy.svg)](https://dashboard.balena-cloud.com/deploy?repoUrl=https://github.com/lumeohq/lumeod-balena-jetson&defaultDeviceType=jetson-xavier-nx-devkit)

Once the application is running on your Balena device, it will auto register with Lumeo and appear in your Lumeo console.

## Setup Jetson Hardware
Finally, log into your hardware device and set max perf mode : 

```.bash
$ balena ssh <device-uuid>

# NVP Mode 8 sets Jetson NX to 20W, 6 core (max perf).
$ nvpmodel -m 8
```
