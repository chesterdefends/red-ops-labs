# ProLUG Linux Sys Admin Course

Course: [https://professionallinuxusersgroup.github.io/lac/](https://professionallinuxusersgroup.github.io/lac/)
Labs: [https://killercoda.com/](https://killercoda.com/)

---

## Resume additions after finishing this course:

```

1. Explain the server build process and hardware system components.
2. Analyze system security and implement basic hardening of system.
3. Construct command line syntax to explore the system and gather resource information.
4. Construct scripting structures of assigning variables, conditional tests, and recording output to generate
    scripts that do basic system tasks.
5. Analyze and troubleshoot the Apache Web Server
6. Analyze and troubleshoot the NFS/Samba File Shares.
7. Analyze Docker and Kubernetes components and workflows.
8. Describe and troubleshoot network services.
9. Write and perform Ansible tasks to automate deployments to servers.
```

1. Build and configure a Linux system to adhere to compliance frameworks.
2. Integrating Linux to a network in a secure fashion.
3. Integrating Linux with Enterprise Identity and Access (IAM) frameworks.
4. Implement User ingress controls to a system/network with bastion frameworks.
5. Updating Linux to resolve security vulnerabilities and reporting out to security teams.
6. Design logging workflows to move event logging off of systems for real time monitoring.
7. Monitoring and alerting on events in Linux.
8. Maintaining system configuration and remediating drift.

When you're talking security: It's always the keywords.

```
Compliance frameworks: CIS, STIGs, HIPAA, GDPR, FERPA.
```

If they reach out to you for an interview, you can always review the standards (you would on the job anyway) and discuss how we integrated them into our system design and deployments.

---

**Bash commands help:**

Find commands you need using:

- `apropos <keyword>` - Each manual page has a short description available within it. `apropos` searches the descriptions for instances of `keyword`. This helps to find if a command will suit your needs.
    - `apropos -s <section_number> <keyword>` - Limit the search of `keyword` to section `<section_number>` of the manpages.
- Read this short refresher on Linux Man Pages and how they work, how sections work in manpages, how `man` command works! Before proceeding!
    - `man <command>` - Search for a particular command in all manpages and display the first instance
        - `man -f <command>` or `whatis <command>` - Searches for the `<command>` across all sections of the manual and lists the results with the corresponding section number and the one-line description from each manpage.
            - `man <manpage_section_number> <term>` - Search for `<term>` in section `<manpage_section_number>` of the manpage and display it. Must for opening specific commands like `adduser(8)`, where `8` is the `<manpage_section_number>`.
- `whereis <command>` - Lists paths of where the command is installed and paths of its corresponding manpages.
- `which <command>`shows the exact location of a given executable. Only works for executable programs

[Unit 1: Command-Line Fundamentals](Unit%201%20Command-Line%20Fundamentals%201c43ea4e1c73800282e5e1445e2f8126.md)

`touch`

`echo “ ” > <filename>` vs `echo “ ” >> <filename>`