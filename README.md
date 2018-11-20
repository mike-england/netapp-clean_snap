# netapp-snapclean
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

## License
This project is licensed under the GNU General Public License v3 - see the LICENSE.md file for details
