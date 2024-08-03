#!/bin/bash
# Codys rsync backup script
# 2019-04-01
#

UZR=$(getent passwd 1000 | awk -F':' '{print $1}')
Pdate=$(date '+%m-%d-%Y')
Buplog="/home/${UZR}/Desktop/${Pdate}".backup
ROOTdev="/dev/sdd1"
HOMEdev="/dev/sdd3"

	touch "${Buplog}"
	exec > >(tee -i "${Buplog}") 2>&1

	echo "${USER} ran backup script today."
	echo "time: $(date '+%H:%M:%S')"  "date: ${Pdate}"
	echo

#### ASSURE REMOVABLE BACKUP DRIVE IS AVAILABLE AND PROPER UUID. EXIT SCRIPT IF NOT TRUE. ####

Root=$(lsblk -no UUID "${ROOTdev}")
Home=$(lsblk -no UUID "${HOMEdev}")

if	! [[ (${Root} = ecdd4c65-7a93-4be9-9ebf-d201a34dbe3d) && (${Home} = ea3ade3a-cb50-4ef2-afdd-ff755ca87398) ]]; then
	echo ; echo "Destination device unavailable, aborting backup." ; echo
	chown "${UZR}" /home/"${UZR}"/Desktop/*.backup	
	exit
fi

	mkdir -p /mnt/root
	mkdir -p /mnt/home

	mount "${ROOTdev}" /mnt/root || exit
	mount "${HOMEdev}" /mnt/home || exit


	echo; echo; echo "Files Transfered And Deleted From Destination Root:" ; echo

	
# rsync -aAHXv --dry-run --delete --exclude={/etc/fstab,/home/*,/dev/*,/proc/*,/sys/*,/tmp/*,/var/tmp/*,/run/*,/mnt/*,/media/*,/root/.cache/thumbnails/*,/lost+found} / /mnt/root
  rsync -aAHXv           --delete --exclude={/etc/fstab,/home/*,/dev/*,/proc/*,/sys/*,/tmp/*,/var/tmp/*,/run/*,/mnt/*,/media/*,/root/.cache/thumbnails/*,/lost+found} / /mnt/root


	echo; echo; echo "Files Transfered And Deleted From Destination Home:" ; echo


# rsync -aAHXv --dry-run --delete --exclude={"${UZR}"/.thumbnails,"${UZR}"/.mozilla/firefox/,"${UZR}"/.cache/mozilla,"${UZR}"/.cache/google-chrome,"${UZR}"/.cache/thumbnails,"${UZR}"/.local/share/Trash/,.Trash-0/} /home/ /mnt/home
  rsync -aAHXv           --delete --exclude={"${UZR}"/.thumbnails,"${UZR}"/.mozilla/firefox/,"${UZR}"/.cache/mozilla,"${UZR}"/.cache/google-chrome,"${UZR}"/.cache/thumbnails,"${UZR}"/.local/share/Trash/,.Trash-0/} /home/ /mnt/home


	chown "${UZR}" /home/"${UZR}"/Desktop/*.backup

	echo
	umount "${ROOTdev}" || echo "Problem with unmounting ${ROOTdev}"
	umount "${HOMEdev}" || echo "Problem with unmounting ${HOMEdev}"


# Backup to nas, ran manually,

# rsync -aAHXv --delete --exclude={/etc/fstab,/home/*,/dev/*,/proc/*,/sys/*,/tmp/*,/var/tmp/*,/run/*,/mnt/*,/media/*,/root/.cache/thumbnails/*,/lost+found} / user@ip-address:/backup/root
# rsync -aAHXv --delete --exclude={"${UZR}"/.thumbnails,"${UZR}"/.cache/mozilla,"${UZR}"/.cache/google-chrome,"${UZR}"/.cache/thumbnails,"${UZR}"/.local/share/Trash/,.Trash-0/,.cache/mozilla,.cache/thumbnails,.thumbnails} /home/"${UZR}"/ user@ip-address:/backup/home/"${UZR}"
