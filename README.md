# lumeod-balena-jetson

Base app/image that you can install lumeod on. Work in Progress.

1. Push to your fleet using :

```
balena push <fleet-name>
```

2. Then log into the installed app container : 

```
balena ssh <device-uuid> lumeo
```

3. and run the lumeo installer. The installer requires manual workarounds to work at the moment.

```
bash <(wget -qO- https://link.lumeo.com/setup)
```

- Wait for the installer to hang trying to enable docker.
- Terminate it (Ctrl C)

4. Unzip the installer and edit the install script

```
sh lumeod-0.3.6-installer-aarch64-linux.run --target ./lumeo
```
- Installer starts again; terminate it. 

5. Edit `setup.sh` :
- Comment out line 231 : `#systemctl enable --now docker`
- Update lines 234-239 to : 
 ```
 docker run -dit --network=host --restart=always \
  -e RUST_LOG=debug \
  -e GST_DEBUG=2,webrtcbin:4 \
  -e WEBRTCD_PIPELINE_PATH="/lumeo_bin/bin/lumeo-webrtcd-pipeline" \
  -v "${BALENA_APP_ID}_lumeo-bin:/lumeo_bin" \
  ${GSTREAMER_DOCKER_IMAGE} /lumeo_bin/bin/lumeo-webrtcd
 ```
- Comment out line 254 : `#install -m u=rwx,g=rx,o=rx ./update.sh /etc/cron.hourly/lumeo-update`

6. Run `setup.sh aarch64` again
7. To set it up as a new gateway, now run :
 ```
CONFIG=/var/lib/lumeo/lumeod.toml
sudo lumeod provision
sudo chown lumeod:lumeod "$CONFIG"
```

8. Start services 
```
service lumeod restart
service lumeo-rtspd restart
systemctl status lumeod
```
  
