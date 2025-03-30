# 5. Basic User Management

Exam Obj:

1. Create, delete, and modify local user accounts
2. Change passwords and adjust password aging for local user accounts (only first part is covered in this chapter—”change passwords”)

| Command | Use | Syntax | Switches |
| --- | --- | --- | --- |
| `who`  OR   `w`  | List logged-in users |  |  |
| `last` | List all user: login/out & sys reboot occurrences |  | `reboot` - list reboot details only |
| `lastb` | List history of unsuccessful login attempts |  |  |
| `lastlog` | List most recent login attempts for every user account |  |  |
| `id` OR `groups` | List user and/or group info |  |  |
| `useradd` | Add new user |  |  |
| `usermod` | Modify attributes of an existing user |  | `-l` = adjust login name
`-u` = adjust UID
`-m -d` =  move and create new home directory
`-s` = adjust login shell  |
| `userdel` | Remove user from system |  |  |
| `passwd` | Set or modify a user’s password |  |  |
| Ctrl+C | Cancel out of a command |  |  |
| `su <login name>`  | Login as desired user |  |  |
| Ctrl+D | Log out of a user account |  |  |
| `usermod -l user2new -m -d /home/user2new -s /sbin/nologin -u 2000 user2`
 | Modify user attributes (of user2): 
Login Name: user2new
UID: 2000
Home directory: /home/user2new
Login shell: /sbin/nologin |  |  `-l` = adjust login name
`-u` = adjust UID
`-m -d` =  move and create new home directory
`-s` = adjust login shell  |
| `grep <username> /etc/passwd` | Search for user information in passwd file |  |  |
| `cd /etc ; grep <username>: passwd shadow group gshadow` | Search for user information in all (4) authentication files |  |  |

Types of users (3): root, user, service

User account information is stored in (4) files (AKA user authentication files): 

/etc….

1. **/passwd** = user login data

![image.png](image%203.png)

1. **/shadow** =user authentication and password aging information
- 

![image.png](image%204.png)

1. **/group**
2. **/gshadow** = contains hashed group-level passwords
- `gpasswd`  command is used to add group admins, add/delete group members, assign or revoke a group-level password, etc.

- UIDs between 1-200 are for core service accounts (201-999 for non-core service accounts)
- /home/<user login name> = their home directory by default

Configuration Files (for default user options): 

*/etc/default/**useradd***   &  *etc/**login.defs***

No-Login (Non-Interactive) User Account: 

The nologin shell (***/sbin/nologin***) is a special program used for user accounts that do not require login access to the system. When a user that is assigned this shell program tries to login, they will be refused with the message “This account is currently not available.”  

Typical examples of accounts that don’t require login access: *ftp, mail,* & *ssh*