# 9. Basic Package Management

**Exam obj:**

1. Install and update software packages from: 
- Red Hat Network—a remote repository
- the local file system (part of this objective is also covered in chapter 10)

| Command | Use | Switches | Syntax |
| --- | --- | --- | --- |
| `rpm`  | Installation and administration of RPM (.rpm) packages |  |  |
|  |  | To query (installed packages from) the RPM database:
`-qa` = shows all packages currently installed
`-qf` = shows where (i.e. which package) a file was installed from
`-ql` = lists files from a package
`-q -- scripts` =shows scripts executed while installing a package
`-q --changelog` = show the change log for a package |  |
|  |  | To query package files (before you install them): 
Add `-p` to any command above |  |
| `rpm2cpio <package_name> | cpio -tv`  = will show contents of a package
`rpm2cpio <package_name> | cpio -idmv`  = extracts the package contents to current directory | To extract an RPM Package (to current directory without installing) | 

 |  |
| 1) `which <file_name>`
ex —> `which ls`
2) `rpm -qf <binary path to file>` 
ex —> `rpm -qf /bin/ls` | Use these two commands to find out where (i.e. which package) a file was installed from  |  |  |
|  |  |  |  |
|  |  |  |  |
|  |  |  |  |
|  |  |  |  |
1. Go to VirtualBox VM Manager and make sure that the RHEL 9 image is attached to RHEL9-VM1: 
- Go to VirtualBox Manager —> Settings —> Storage
- Under ‘Controller: IDE’ click the disk image displayed
- Under Attributes —> Optical Drive click the disk icon to the right to and choose the rhel-9 .iso image listed there (this will attach the correct disk file to the optical drive)
-