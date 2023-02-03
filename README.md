# ros_upgrade

Upgrade RouterOS packages and firmware

This role handles RouterOS Versions and firmware, both upgrade and downgrade. Upgrades can be done via SFTP push, or via the built-in package update on the MikroTik. Firmware will always be set to match the RouterOS version, as per MikroTik reccomended best practices. Please deploy from tags, not the master branch. Master branch should be considered experimental.

Note - Requires the community.routeros collection. Install via `ansible-galaxy collection install -r collections/requirements.yml`

* Not suggested for use to switch to testing channel as there could be breaking changes in v7.

# Download Method
* Note, this method will not handle downgrades, just upgrades to the latest release in the `update_channel` set by this job. (stable, LTS, beta, testing)

1. Set the `install_method` variable to `download`
2. Set the `update_channel` variable to your desired channel (stable, long-term, development, testing)
3. `routeros_version` is not evaluated using Download Method, but will affect validation checks if you want this.
4. (optional) Adjust the `reboot_timeout` variable if you have slower connections or waiting for reboots is timing out
5. (optional) set `validation` variable to true to verify all upgrades after processing. (needs to have routeros_version set also)
6. Run the job. (Devices will be upgraded, rebooted, firmware upgraded, and rebooted once more.)

# Push Method
* Note, this method can be used to set a specific version of RouterOS. (will handle upgrade or downgrade to achieve desired version)

1. Set the `install_method` variable to `push`
2. Set the `routeros_version` variable to the latest version
3. Set the `update_channel` variable to the desired channel (not actually used but should be set for consistency)
4. (optional) place the .npk files into the `packages/` directory. (Otherwise Ansible will use your hosts internet connection to download them.)
5. (optional) set `validation` variable to true to verify all upgrades after processing.
6. Run the job. (Devices will be upgraded, rebooted, firmware upgraded, and rebooted once more.)


# Disclaimer

Please do your own testing with this. I have tested with most of the 6.48 (stable), and 6.47 (LTS) trains, but there are no guarantees. Please understand what this job is doing and what impacts it might have on your environment.
