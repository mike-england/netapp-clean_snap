# netapp-clean_snaps
Have you ever had lingering snapshot taking up space because of a policy change or failed automation process?  This is a time saver script to clean up old snapshots from a volume, set of volumes, or entire vserver.

## Getting Started
This is a simple, self contained script, just download it, place it wherever you like and run to show help information

```
$ chmod +x clean_snaps
$ ./clean_snaps

clean_snaps -u [user_name] -c [cluster] -vs [vserver] -v [volume] -d [days_to_keep] [-r] [-np]
	-u is the username to log into the storage array with, e.g. domain\\mengland
	-c is the cluster mode system to attach to, e.g. my-cluster.domain.com
	-vs is the vserver hosting the volume, e.g. vserver1
	-v the volume you want snaps to be deleted from.  This can include * for group matches as long as it's escaped from the command line
		E.g. vol_mike\* for anything starting with vol_mike, or \* for everything on the vserver
	-d the number of days to keep snaps, all others will be deleted.
		E.g. -d 2 will keep today - 2 days.  If today were the 10th, it would keep the 8th, 9th, and 10th deleting everything prior.
	-r indicates a report only, don't actually delete anything
	-np will suppress any progress messages

```

## Usage Examples
Report only for all snapshots older than 2 weeks
```
$ ./clean_snaps -u ad\\mengland -c labcluster1.mydomain.com -vs lab-vserver1 -v \* -d 14 -r
10:57:54 PST - Reporting Only
Password for user ad\mengland:
10:58:14 PST - ------ Clean Snap "esx_volume_ds01" ------
10:58:14 PST - Looking for snapshots older than 0 days on volume esx_volume_ds01
10:58:14 PST - ssh labcluster1.mydomain.com -l ad\mengland snap show -vserver lab-vserver1 -volume esx_volume_ds01 -create-time !*"Nov 19"*,!*"Nov 18"*,!*"Nov 17"*,!*"Nov 16"*,!*"Nov 15"*,!*"Nov 14"*,!*"Nov 13"*,!*"Nov 12"*,!*"Nov 11"*,!*"Nov 10"*,!*"Nov 09"*,!*"Nov 08"*,!*"Nov 07"*,!*"Nov 06"*,!*"Nov 20"* -fields vserver,volume,snapshot,create-time
10:58:15 PST - Snapshots matching criteria:
lab-vserver1 esx_volume_ds01 hourly.2015-03-30_1805 Mon Mar 30 18:05:00 2015 
lab-vserver1 esx_volume_ds01 hourly.2015-03-30_1905 Mon Mar 30 19:05:00 2015 
```
To delete these old snapshots, run the same command without -r
```
$ ./clean_snaps -u ad\\mengland -c labcluster1.mydomain.com -vs lab-vserver1 -v \* -d 14
11:01:52 PST - ------ Clean Snap "esx_volume_ds01" ------
11:01:52 PST - Looking for snapshots older than 0 days on volume esx_volume_ds01
11:01:52 PST - ssh labcluster1.mydomain.com -l ad\mengland snap show -vserver lab-vserver1 -volume esx_volume_ds01 -create-time !*"Nov 19"*,!*"Nov 18"*,!*"Nov 17"*,!*"Nov 16"*,!*"Nov 15"*,!*"Nov 14"*,!*"Nov 13"*,!*"Nov 12"*,!*"Nov 11"*,!*"Nov 10"*,!*"Nov 09"*,!*"Nov 08"*,!*"Nov 07"*,!*"Nov 06"*,!*"Nov 20"* -fields vserver,volume,snapshot,create-time
11:01:54 PST - Snapshots matching criteria:
lab-vserver1 esx_volume_ds01 hourly.2015-03-30_1805 Mon Mar 30 18:05:00 2015 
lab-vserver1 esx_volume_ds01 hourly.2015-03-30_1905 Mon Mar 30 19:05:00 2015 
11:01:54 PST - Asked to delete snapshots:
hourly.2015-03-30_1805,hourly.2015-03-30_1905
11:01:54 PST - Deleting snapshots on volume esx_volume_ds01, press Control-C to abort
5.4.3.2.1.
```
You'll notice the script will give a list of snapshots it's going to delete and a 5 second countdown in case you see something you don't like, you can press control-c to stop the script.

A password is not accepted as a command line option as it'll remain in your shell history.

## License
This project is licensed under the GNU General Public License v3 - see the LICENSE file for details
