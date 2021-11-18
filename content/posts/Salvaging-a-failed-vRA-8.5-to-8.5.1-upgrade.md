---
title: 'Salvaging a failed vRA 8.5 to 8.5.1 upgrade'
date: 2021-10-25T15:37:40-04:00
draft: false
toc: true
cover: img/vra8-icons.png
tags:
  - vRA
---

## What happened

I ran into an issue while upgrading vRealize Automation (vRA) 8.5 to 8.5.1 using vRealize Suite Lifecycle Manager (LCM) 8.5. This post is just what worked for me and should not be used in ~~a production~~ any environment.

LCM's prechecks make upgrading the vRealize products significantly easier, but unfortunately not foolproof. As of 8.5 the precheck doesn't check for free disk space, in my situation, was running dangerously low due to three log-bundles still hanging around. Those log-bundles take up some space at 6+ Gbs each and were leftovers from previous support requests.

After watching the fun green lines hop around in the Request details view, I got the following error:

```text
Error Code: LCMVRAVACONFIG90030 vRealize Automation VA Upgrade Status Check failed.
Upgrade on vRealize Automation VA apl00617.csxt.csx.com failed with state fatal.
To know more about the failure, run command "vracli upgrade status --details" on the vRealize Automation appliance.
If user wants to revert snapshot and trigger upgrade again,
click RETRY with revertSnapshotNRetryUpgrade property set to true (or)
If user wants to cancel the whole uprgade and revert to the state before upgrade click RETRY with cancelUpgradeNRevertBack property set to true.
If both the retry properties are set to true, revertSnapshotNRetryUpgrade property will take precedence and will be honoured
```

## Steps

### The short answer

If you took (and still have) a snapshot of the appliance, like LCM asks you to do, you can just revert back to it, clean up the log-bundles and be back on your way.

However if for some reason you don't have a backup available, then there are a good deal more steps to take.

### Clean up the log-bundles and check space

After removing the log-bundles, double check that there is nothing else taking up too much space with `lsblk`.

```shell
NAME           MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda              8:0    0   60G  0 disk
├─sda1           8:1    0    2M  0 part
├─sda2           8:2    0  128M  0 part /boot
├─sda3           8:3    0    1G  0 part
└─sda4           8:4    0 58.9G  0 part /
sdb              8:16   0  144G  0 disk
└─data_vg-data 254:0    0  144G  0 lvm  /data
sdc              8:32   0   22G  0 disk
└─logs_vg-log  254:1    0   22G  0 lvm  /var/log
sdd              8:48   0   20G  0 disk
└─home_vg-home 254:2    0   20G  0 lvm  /home
sr0             11:0    1 1024M  0 rom
```

### Take a snapshot of the appliance with LCM

Take a snapshot now because you wish you had one earlier and you might need one later on.

### Make a backup of object.yaml

```shell
cp /data/restorepoint/sys-config/vaconfig/object.yaml ~/
```

### Delete everything in /home/root/

```shell
rm -rf /home/root/
```

### Delete and create a few soft links

Paste in the whole block

```shell
cp -r /root/ /home/root
sed -i '/root/s!\(.*:\).*:\(.*\)!\1/home/root:\2!' /etc/passwd
sleep 10
rm -rf /root && ln -s /home/root /root
pwconv
rm -rf /metrics && ln -s /home/metrics /metrics
```

### Run through the update scripts one at a time

Some of them don't have an output, for those check the if they completed successfully with `echo $?`

```shell
 cd /etc/bootstrap/postupdate.d/
```

```shell
 ./02-10-load-images.sh
```

```shell
  ./02-disable-var-log-cron.sh
```

```shell
  ./03-05-enable-metrics.sh
```

```shell
  ./03-10-setup-k8s.sh
```

```shell
  ./10-35-fix-named.sh
```

```shell
  ./10-disable-tzselect.sh
```

```shell
  ./20-relocate-db.sh
```

```shell
  ./71-11-remove-pr2709935-crons.sh
```

```shell
  ./91-00-remove-config-xl.sh
```

### Reboot the appliance

```shell
reboot
```

### After it boots back up, check the status

```shell
vracli vidm
```

```shell
vracli status first-boot
```

### Apply the object.yaml

```shell
kubectl apply -f /data/restorepoint/sys-config/vaconfig/object.yaml
```

### Run the deploy script

```shell
/opt/scripts/deploy.sh
```

Then get a coffee, because this takes around 20+ minutes.

### Check the version

If everything worked as expected, you should see 8.5.1.

```shell
vracli version
```

### Clean up leftovers so the appliance can be upgraded later on

```shell
cd /var/vmware/prelude/upgrade
```

```shell
rm -rf /data/restorepoint /var/vmware/prelude/upgrade /var/log/vmware/prelude/upgrade-report-latest*
```

### Check the upgrade status

```shell
vracli upgrade status
```

## Conclusion

Hopefully this helps if you somehow found yourself in the same situation I did.
