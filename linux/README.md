### This file [linux] shows some of my abilities as Linux System Admin  

---

Linux Systems Administration

Step 1: Ensure/Double Check Permissions on Sensitive Files
	1. Permissions on /etc/shadow should allow only root read and write access.
		Command to inspect permissions:
			ls -l shadow

		Command to set permissions (if needed):
			sudo chmod 600 /etc/shadow

	2. Permissions on /etc/gshadow should allow only root read and write access.
		Command to inspect permissions:
			ls -l gshadow

		Command to set permissions (if needed):
			sudo chmod 600 /etc/gshadow

	3. Permissions on /etc/group should allow root read and write access, and allow everyone else read access only.
		Command to inspect permissions:
			ls -l group

		Command to set permissions (if needed):
			sudo chmod 644 /etc/group

	4. Permissions on /etc/passwd should allow root read and write access, and allow everyone else read access only.		
		Command to inspect permissions:
			ls -l passwd

		Command to set permissions (if needed):
			sudo chmod 644 /etc/group


Step 2: Create User Accounts
	1. Add user accounts for sam, joe, amy, sara, and admin.
		Command to add each user account (include all five users):
			sudo adduser sam
			sudo adduser joe
			sudo adduser amy
			sudo adduser sara
			sudo adduser admin

	2. Ensure that only the admin has general sudo access.
		Command to add admin to the sudo group:
			sudo usermod -aG sudo admin


Step 3: Create User Group and Collaborative Folder
	1. Add an engineers group to the system.
		Command to add group:
			sudo addgroup engineers

	2. Add users sam, joe, amy, and sara to the managed group.
		Command to add users to engineers group (include all four users):
			sudo usermod -aG engineers sam
			sudo usermod -aG engineers joe
			sudo usermod -aG engineers amy
			sudo usermod -aG engineers sara

	3. Create a shared folder for this group at /home/engineers.
		Step 1. Create the folder to be shared
			udo mkdir /home/engineers
		Step 2. Create a user group
			sudo addgroup engineers - completed before 3.1
		Step 3. Give permissions
			sudo chmod 660 /home/engineers
				Check the result:
					ls -l
		Step 4. Add users to the group
			sudo usermod -aG engineers sam - completed before 3.2
			sudo usermod -aG engineers joe - completed before 3.2
			sudo usermod -aG engineers amy - completed before 3.2
			sudo usermod -aG engineers sara - completed before 3.2
				Check the result:
					groups sam joe amy sara 
	
		Command to create the shared folder:
			sudo chmod 660 /home/engineers
			NOTE: will give Owner and group W R access

	4. Change ownership on the new engineers' shared folder to the engineers group.
		Command to change ownership of engineer's shared folder to engineer group:
			sudo chown :engineers ./engineers

Step 4: Lynis Auditing
	1. Command to install Lynis:
		sudo apt install lynis

	2. Command to see documentation and instructions:
		man lynis

	3. Command to run an audit:
		sudo lynis audit [service] or [groups]
			example :
				sudo lynis audit system
				sudo lynis audit --tests-from-group accounting, banners, firewalls, kernel >> [addit to secific location]

			The following command can be used to display the lynis groups:
				sudo lynis audit show groups
					Example: 
						sudo lynis audit system


	4. Provide a report from the Lynis output on what can be done to harden the system.
		Linus gives you suggestions on what needs to be done in your system or the group you selected.

		In the example below the first warning says the linis needs to be updated.
	
		The 3rd warning shows a vulnerable package, sudo lynis show details PKGS-7392 can be used to get details of what needs to be done to harden the system.
		Screenshot of report output:
			Reports are saved in: /var/log/lynis-report.dat
			Detailed info is logged to: /var/log/lynis.log


Bonus
	1. Command to install chkrootkit:
		sudo apt install chkrootkit

	2. Command to see documentation and instructions:
		man chkrootkit
	
	3. Command to run expert mode:
		sudo chkrootkit -x | more   

chkrootkit looks for known "signatures" in trojaned system binaries. The expert mode will identify possible system issues.

	4. Provide a report from the chrootkit → typo chkrootkit output on what can be done to harden the system.
		Screenshot of end of sample output:
		

		As a system administrator an script can be created to run chkrootkit and email the result using a cron job. The following code taken from gibHub is a good example of what can be done to harden the system.


			#!/bin/bash
			echo “If you feel chkrootkit is outdated, update it manually ;)” > /tmp/chkrootkit
			echo ” ” >> /tmp/chkrootkit
			echo “Scanning the system with ChkRootkit” >>
			/tmp/chkrootkit/usr/local/chkrootkit/chkrootkit >>
			/tmp/chkrootkit cat /tmp/chkrootkit | mail -s “ChkRootkit scan report on `hostname`” <email address>

		#Save this script [ /root/chkhunter-weekly.sh ], then set a cron to run this script every sunday at 15:00 hours.
			0 15 * * 0 /root/chkrootkit-weekly.sh