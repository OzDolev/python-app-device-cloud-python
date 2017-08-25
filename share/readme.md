# Share
## Folder Contents
 * `hdc-dev-mgr.service` - Example systemd service configuration file
 * `hdc-dev-mgr.sh` - Example init.d service script
 * `create_ota_package.sh` - Python script that will generate OTA packages

## Running the HDC Device Manager as a Service
Prior to using either the systemd or init.d files to setup the device manager
as a service (as described below), you must first make sure that you have the
dependencies installed in the correct place. These modules should be in the
system Python packages folder, not your local one. To do this, you can run:
`sudo pip install --target=/usr/lib/python2.7/dist-packages/ /path/to/wr-iot-python`

### Using the HDC Device Manager with Systemd
 `hdc-dev-mgr.service` is an example systemd unit file that will start the HDC
 Python Device Manager at boot and restart it if it crashes. To use it:
 
 1. Update the paths in the file to point to your installation (for the most
    part, just change the parts that say `/path/to/`). *Note: if you are using
    /var/lib/iot as your runtime directory, you will need to run the device
    manager as the `iot` user in order to have write permissions To do this, 
    add `User=iot` to the `[Service]` section of `hdc-dev-mgr.service`.*
 2. Copy the file to the systemd unit file directory. On Ubuntu, this is
    `/etc/systemd/system/`. You will need root access to do this.
 3. Enable the service unit in systemd by running
    `sudo systemctl enable hdc-dev-mgr`.

 You can now use `systemctl start` and `systemctl stop` to manually start/stop
 the service. `systemctl status` will indicate if the service is running or not.
 In addition, the service will run automatically when your system starts.

### Using the HDC Device Manager with init.d
 `hdc-dev-mgr.sh` is an example init.d script that will start the HDC Python
 Device Manager at boot. To use it:
 
 1. Update the path at the top of the script (change `/path/to/...` to
    point to your installation)
 2. Copy the file to the init.d directory, `/etc/init.d/` on most systems
 3. Enable the service. The command to do this will depend on your distro,
    but here are two examples:

    * Ubuntu: `update-rc.d hdc-dev-mgr.sh defaults`
    * Centos:  `chkconfig --level 345 hdc-dev-mgr.sh on`

 You can now use `service hdc-dev-mgr` to control the service. For example,
 `sudo service hdc-dev-mgr start` will start the service. The service will also
 run automatically when your system starts.

 *Note: Most modern full-featured Linux-based operating systems use systemd to
 control the legacy init.d scripts. If you're using a system that has systemd,
 it is recommended you use the specific systemd service file as described above*

## Using the OTA Package Generator
The OTA package generator allows you to create OTA update packages for your
devices. It can be run with or without parameters and can be run in a
"headless" mode to allow for fast package generation via the command line.
See `create_ota_package.py --help` for a list of valid parameters.
