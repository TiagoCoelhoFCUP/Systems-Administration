# Systems-Administration
Projects developed under the Systems Administration college chair during the 2019/2020 school year


## LDAP/NFS-based Directory Information System

The objective of this project is the definition and implementation of an LDAP/NFS-based directory information system for authentication and export of directories, according to the following picture:

<p align="center">
  <img src="https://user-images.githubusercontent.com/13381706/163480610-a5d4d625-150f-4c19-aa57-b6cdf0a7ccd4.png" alt="Sublime's custom image"/>
</p>

### LDAP 

It is through OpenLDAP, an open source implementation of LDAP, that the server (10.0.0.1) will be able to authenticate users on the Desktop and Win10 machines (10.0.0.2 and 10.0.0.3 respectively), as well as know which directories to mount for the authenticated user.
Using a sample configuration as a base, the root and Manager passwords were configured, olcSuffic changed to dc=admins,dc=ads,dc=dcc, as well as the Manager DN to comply with the new suffix.  

<p align="center">
  <img src="https://user-images.githubusercontent.com/13381706/163480748-2e21c785-b76b-47c3-9bbc-38ad7a9dec6f.png" alt="Sublime's custom image"/>
</p>

### RAID

We decided to use a RAID 10 device, or RAID 1+0, because this type of configuration combines two features that we identified as important: The mirroring and striping of the disks.
RAID 10 works like a RAID 0 of two RAID 1 devices, where information is written alternately between each of the RAIDs 1 and within RAID 1 it is duplicated by the 2 disks. As long as one disk in each mirrored pair is operational, the data can be recovered. If two disks in the same mirrored pair fail, all data is lost.
The RAID device was created on omv (10.0.0.4) with 4 disks: /dev/sdb, /dev/sbc, dev/sdd, dev/sde, each with 1GB capacity and was given the name rd10.

<p align="center">
  <img src="https://user-images.githubusercontent.com/13381706/163481175-2f4daa7c-fabf-4930-86db-5a1d81a35f18.png" alt="Sublime's custom image"/>
</p>


### NFS

NFS was configured as follows:
<ul>
  <li> The rd10 device was formatted with an ext4 file system and then mounted. </li>
  <li> A new share was created, named radi-sh, using the RAID-based filesystem with write and read access permissions to users and only from machines on the 10.0.0.0/24 network. </li>
  <li> On the Desktop, this new share was added to /etc/fstab and mounted in the /net/home directory </li>
</ul>
  
Next, to make it possible to export the homes of the LDAP authenticated users, their home directories were created in this omv share and the ownership and access permissions of the directories in question were changed, in order to map them correctly with the LDAP uid and gid.

For more detailed information about the project make sure to read the full report.
