# Systems
Systems Troubleshooting

Server Triage Checklist	1
Pre-verification	1
Fusion IO	2
Logical Drive Fix	2
Emergency Mode Fix	3
GRUB Fix	3
FlexLOM Fix	4
HP RESTful API Issues	4
Gold Memory Installation	4
HP Case Creation Template	4
Running UEFI Test on HP Server	5
Server Triage Checklist
The following checklist is used to ensure we check for all potential system health issues as well as verifying/ensuring the latest approved firmware and tools are installed on the servers.

Check hardware health status.  Replace components as needed.
FlexLOM
Storage Array Controller
Hard Drive
Storage Cache Battery
Check firmware versions of applicable server components.  Update as needed.
iLO
FlexLOM
Storage Array Controller
Hard Drive
Storage Cache Battery
Check server tools to ensure they are installed.  List of required tools below.
Hpssacli
Hp-health
Hplog
Dmidecode
yum
Pre-verification
For all troubleshoot request, DCOPS must verify the server is ready for maintenance and if the machine requires silencing in Sensu.  If there is any question if it is okay to begin work, the DCOPS engineer should contact the ticket reporter for confirmation.  Once given the okay to proceed, the DCOPS engineer must then verify that the alert is silenced in Sensu.  https://sensu.addsrv.com

Log into Sensu with your AD credentials
Search for the target device by the fully qualified domain name
Ex. ids-1-003.shared.security.las1.mz-inc.com
Verify the alert has been silenced
The speaker will noted with a red marker

If the alert is silenced, troubleshooting can begin.
If the alert is not silenced, verify with the ticket reporter that the system is still okay to work on and have the alert re-silenced.
Fusion IO
For fusion IO troubleshoot tickets, the fio-bugreport is required to be sent to HPE upon case creation.  This report should already be uploaded to the ticket by the reporter.  If not found attached to the JIRA ticket, reach out to the reporter to have them uploaded.  This report is to be uploaded to the HPE ftp site dedicated to MZ and placed in the directory named after the HPE case number.

Run the following commands from the HP CLI
Fio-status
This provides the serial number of the Fusion IO card and any errors.
Sosreport
This is a report required by HPE to be analyzed prior to issuing a replacement RMA part.  This report is to be uploaded to the ftp site in the same directory as the fio-bugreport.
AHS log
This is downloaded directly from the server via iLO.  This report is to be uploaded to the ftp site in the same directory as the fio-bugreport.
Open HPE support case 
Copy the output of the fio-status report into the HPE case details
Logical Drive Fix
For tickets citing bad logical drive(s)...

Reboot server and press F10 (Intelligent Provisioning) when prompted.
Click 2nd option (SSA - Smart Storage Administrator).
Once in the Smart Storage Administrator proceed to step 3.
Find and select the failed controller in the left column. 
In the middle column, choose <configure>.
In the left column, choose <logical drives>.
Scroll down on the logical drives and click on the failed logical drive. 
In the right column, click on <enable logical drive>.
<Reboot>

Note: If 1 or more logical drives have failed on different controllers, repeat the above steps for each controller.
Emergency Mode Fix
For servers that boot into emergency mode…

When the server is in emergency mode log into the current screen with the root password.
For servers with Fusion IO cards that are corrupt, you will need to go to the file system (vi /etc/fstab) comment out /fioa
(example  #/fioa and save the file in VI. Then reboot the server.
For servers without fusion cards follow these steps…
Log into the server in emergency mode and run this command hpssacli ctrl all show config. look for any failed drives
Make sure you see all the drives in the system
Verify the block by running lsblk
After running lsblk count all the drives and any missing ones you can comment them out.
Example…
ROOT@mphdp-1-027.live.mp.las1:~
$ tail /etc/fstab
#LABEL=data16 /data16 xfs rw,noatime,nodiratime,nobarrier 1 2
LABEL=data17 /data17 xfs rw,noatime,nodiratime,nobarrier 1 2
LABEL=data18 /data18 xfs rw,noatime,nodiratime,nobarrier 1 2
LABEL=data19 /data19 xfs rw,noatime,nodiratime,nobarrier 1 2
LABEL=data20 /data20 xfs rw,noatime,nodiratime,nobarrier 1 2
LABEL=data21 /data21 xfs rw,noatime,nodiratime,nobarrier 1 2
LABEL=data22 /data22 xfs rw,noatime,nodiratime,nobarrier 1 2
LABEL=data23 /data23 xfs rw,noatime,nodiratime,nobarrier 1 2
LABEL=data24 /data24 xfs rw,noatime,nodiratime,nobarrier 1 2
Save the file in VI  (:wq)
Reboot the server and it should bring you to the log on prompt.
GRUB Fix
For servers that boot to GRUB prompt
When the server is in Grub type reboot.  Enable hot keys and select F11 during post.
When the one time boot menu shows up Select the OS (CentOSlinux). 
If the server still does not boot up to the log in screen you can try to run fsck and look for any errors.
Finally if all the steps above have failed open a AUTO ticket and have them investigate.
FlexLOM Fix
Verify POS mac of flex lom which is ENO49 against mac in ILO under system information then select network and  look for Adapter 2.  Port 1 is ENO49  (See Example Below)

Check link lights on nic (solid green means 1G speed)
Log into the switches if you have access then you can bounce the ports and see if they come back up as 10g)
From the CLI run ifconfig and make sure you see ENO49 and ENO50 interfaces and Bond0.  If you do not see one of these open a AUTO ticket and have them investigate.
Replace flex lom if necessary.
HP RESTful API Issues
Drain flea power on server
Power down server and remove power cables.
Hold in the power button on the front of the server for 30 seconds
Reboot server and on post make sure RESTapi is checked.  If you don't see the RESTapi check after a second reboot then you will need to replace the motherboard.
Download AHS logs and Open a HP case to replace the motherboard.
Gold Memory Installation
Format USB drive to FAT32
Please use USB MultiBoot SYSLINUX/FREEDOS Memory Test
Go to the following link to make the USB drive bootable
Download "Rufus" utility, http://rufus.akeo.ie/
Click the disk icon to the right of "Create a bootable disk using:"
Click the disk icon to the right of "Create a bootable disk using:" and select option "ISO Image".  Please use file CDFD798.ISO
(Warning! All Data on the USB Flash Drive will be lost!)
On the server you need to check one time Legacy Boot
When the server boots up, select number 2 (run from usb key)
On the next screen select the first option Gold Memory
WARNING! Testing can take up to 2 weeks to complete.
HP Case Creation Template
Please use this as an example only.  Serial, Model, and Part Number are required information for every case.  If a CE is required, please indicate such in the problem description.  Do not forget to upload all required/applicable logs/reports.

Problem Description
We have a failed drive on one of our servers.  We have replaced the drive with one of our onsite spares.  Please process this as a CSR and include a return shipping label. Please ship us the part. We have uploaded the the logs to the MZ FTP Site.

Serial: USE530TT6C
Model: SPS-DRV HD 1TB 6G SAS 7.2K 3.5 DP MDL SC
Part Number: 653947-001

Running UEFI Test on HP Server
Reboot server and on post hit the F9 key.
Select “Embedded Applications”
Select “Embedded Diagnoistics”
Select option from the menu list to run specific test.
Please see the following  link which gives more detail
http://h20564.www2.hpe.com/hpsc/doc/public/display?docId=mmr_kc-0123691
