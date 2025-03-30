# 10. Adv Package Management

**Exam obj:**

- Install and update software packages from Red Hat Network, a remote repository, or from the local file system

**Exam tip**: Knowing how to configure a dnf repository (i.e. BaseOS or AppStream) using a URL plays an important role in completing some of the RHCSA exam tasks successfully. Use two forward slash characters (//) with the baseurl directive for an FTP, HTTP, or HTTPS source. (ex. **baseurl=file:///mnt/BaseOS)**

Exam tip: Manually configuring repository access is KEY for the exam 

A directive =  the label for a source 

(ex: 

- **name=**
- **baseurl=**
- **enabled=**  )

| Command | Use | Switches | Syntax |
| --- | --- | --- | --- |
| `dnf config-manager --enable <name_of_the_repository>` | To add access to repositories that are offered through subscription manager (provided there is a repository server available) |  |  |
| `dnf config-manager --add-repo=”file:///mnt/BaseOS”
dnf config-manager --add-repo=”file:///mnt/AppStream”`

**OR**

(Manual version)
`sudo vim /etc/yum.repos.d/local.repo`
`[BaseOS] name=BaseOS software baseurl=file:///mnt/BaseOS enabled=1 
gpgcheck=0`
`[AppStream] name=Application software baseurl=file:///mnt/AppStream enabled=1 gpgcheck=0`

*Use manual version if `dnf config-manager` is unavailable* | To create/to add access to repositories using a mount file

To create a new repository file |  |  |
| `ls /etc/yum.repos.d` | List where config-manager creates these repository files |  |  |
| `dnf repolist` | Shows a list of available repositories |  |  |
| `dnf repoquery` | List all packages available for installation from all enabled repos |  |  |
| `dnf repoquery --repo “BaseOS”`

`dnf repoquery --repo “BaseOS” | grep zsh`  | List all packages available for installation from a specific repo

To find whether the BaseOS repo includes the a specific package (zsh) |  |  |
| `dnf repoquery --requires <package_name>` | List all dependencies of a package |  |  |
| `dnf list`

`dnf list "selinux*"`

`dnf list installed`

`dnf list recent` | Lists installed (starts with “@”) and available (starts with “repo_”)  packages

To list whether a package (selinux, for example) is installed or available for installation from any enabled repository

List all installed packages on the system

List any recently installed packages |  |  |
| `dnf search
dnf search all` | Searches in name and summary
Searches in name, summary, and description |  |  |
| `dnf provides */local_path` | Deepest search possible - Search for packages that contain a specific file |  |  |
| `dnf info` | Shows info about the package |  |  |
| `dnf install` | Installs packages as well as any dependencies |  |  |
| `dnf remove` | Removes packages and dependencies |  |  |
| `dnf update`
 | Update all installed packages to the latest available versions |  |  |
| `dnf history` | Displays dnf history |  |  |
| `dnf history undo n` | Undoes desired dnf task  |  |  |
| `dnf group list 

dnf group list installed

dnf group list available` | List all installed and available package groups
List all installed package groups

List all available package groups |  |  |
|  |  |  |  |

It is expected that  you know how to configure access to a dnf repository (BaseOS and AppStream) using a definition file (i.e. a URL)  on the exam. They both come preconfigured with the RHEL 9 ISO image. 

Example of a repo definition file:

[Exclusive ID] 

name=brief description

baseurl=file:///local_path

enabled=1

gpgcheck=0

You would create a repo definition file, entering the above data into it, and placing it in the preferred location to store configuration: **/etc/yum.repos.d** 

## GPG Keys:

- Used to ensure that packages have not been tampered with (package integrity)
- A repository GPG key is used to sign all packages and before installing the package, its signature is checked
- To check using a GPG key, you’ll need a local GPG key to be present
- To make accessing trusted repositories easier, use the gpgcheck=0 option in the repository client file:

`cd /etc/yum.repos.d` —>`vim repo_BaseOS.repo` —> Add `gpgcheck=0` to last line of the file

## Steps to Mount the RHEL installation media and Configure Repository Access:

1. Mount the RHEL installation media (RHEL 9 ISO Image)

`sudo mount /path/to/rhel.iso /mnt/repo`

`sudo mount /path/to/rhel.iso /mnt/repo`

1. Verify that the image is currently mounted: 

`df -h | grep mnt`

1. Create a new repository file:

`sudo vi /etc/yum.repos.d/local.repo`

`[BaseOS] 
name=BaseOS software
baseurl=file:///mnt/BaseOS
enabled=1
gpgcheck=0`

`[AppStream]
name=Application software
baseurl=file:///mnt/AppStream
enabled=1
gpgcheck=0`

1. Clean the yum cache and update repository information:
2. `sudo yum clean all
sudo yum repolist`
    
    
3. Install packages as needed:
    
    `sudo yum install package_name`
    

Using Subscription Manager

- To ___ you need to register and attach your system to the RedHat
1. `subscription-manager register`

(Type in username and password used when registered on RedHat Developer)

1. `subscription-manager attach`