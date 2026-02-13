# 2026-02-07 • **Windows CLI Basics**

| Lab/Room | TryHackMe - Windows CLI |
| --- | --- |
| Type | Classroom |
| Statut | Done |
| Date | 07/02/2026 |

# Context

Explore what Windows CLI is, how to navigate, and interact with the system using Windows CLI.

The **Command Prompt** (often referred to as CMD) is a text-based interface for interacting with the Windows operating system. Instead of clicking folders and menus, you type commands to tell the system exactly what you want to do, such as listing files, moving between folders, or checking system information. It might look simple, but it's a powerful and widely used tool.

# Starting hypothesis

- Use the Windows command line confidently
- Navigate folders without clicking
- Find files when you only know their name
- Read files using the terminal
- Collect basic system and network information

# Method / Used Tools

## Windows CLI: Navigating Files and Finding Your First File

### Terminal (Command Prompt) Basics

The Windows **Command Prompt** is a text-based interface that allows you to interact directly with the operating system. Instead of relying on graphical navigation with a mouse, you execute precise commands to control the system.

In cybersecurity, the terminal is not optional — it is fundamental. It is:

- Faster than navigating through graphical menus
- More precise and controllable
- Required for many administrative and security tools

Whether you are performing troubleshooting, system administration, or penetration testing, mastering the command line is a core technical skill.

### Overview

Before exploring or attacking a Windows machine, you must first understand how to move through its filesystem and interact with files.

The core skills include:

- Navigating between directories
- Listing files and folders
- Searching for specific files
- Reading file contents

These actions form the foundation of system enumeration, incident response, and privilege escalation workflows.

### Where Am I?

To determine your current location in the filesystem, use:

```
cd
```

When used without arguments, the `cd` (Change Directory) command displays the full path of your current working directory.

On Windows systems, this is typically something like:

```
C:\Users\Username
```

Although `cd` is primarily used to move between directories, it also serves as a quick way to confirm your current position before performing any action.

Knowing where you are prevents mistakes, especially when interacting with sensitive files.

### What’s Around Me?

To list the contents of your current directory, use:

```
dir
```

The `dir` command displays:

- Files
- Folders
- Basic metadata such as size and last modification date

This provides immediate situational awareness. Before modifying or reading anything, you should always inspect your environment.

### Are There Hidden Files?

Not all files are visible by default. Some are marked as hidden or system files.

To display all files, including hidden ones:

```
dir /a
```

The `/a` flag tells Windows to show **all** items, including:

- Hidden files
- System files

Important distinction:

Hidden does not mean secret or protected. It simply means Windows does not display the file in a normal listing.

In cybersecurity investigations, checking for hidden files is critical, as attackers often rely on obscurity rather than strong protection.

### Moving Around the Filesystem

To move into a directory:

```
cd folder_name
```

Example:

```
cd Documents
```

To move back one level:

```
cd ..
```

Effective exploration usually combines commands:

- `cd` → change location
- `dir` or `dir /a` → inspect contents

Enumeration is not random clicking — it is structured exploration of the environment.

### Finding a File on the Disk

If you do not know where a file is located, you can search for it using:

```
dir /s task_brief.txt
```

The `/s` flag instructs Windows to:

- Search all subdirectories
- Display the full path of matching files

This is extremely useful during investigations when searching for:

- Configuration files
- Credential files
- Sensitive documents

Instead of manually browsing folder by folder, you automate the search.

### Navigate to the File

Once the full path of the file is known, move to its directory:

```
cd <full_path>
```

Then verify its presence:

```
dir
```

This confirmation step ensures you are in the correct location before interacting with the file.

Precision matters in real-world environments.

### Read the File

To display the contents of a text file directly in the terminal:

```
type task_brief.txt
```

The `type` command prints the file content in the Command Prompt.

This is commonly used to:

- Read configuration files
- Inspect logs
- Extract information during forensic analysis

Reading files from the CLI is often faster and more efficient than opening them in a graphical editor.

## Gathering System Information on Windows

### Overview

After learning how to navigate the filesystem, the next step is system reconnaissance.

Before troubleshooting or launching an attack, cybersecurity professionals first collect essential system information. This helps them understand:

- The current user context
- The machine identity
- The operating system version
- The network configuration

This phase is equivalent to performing a quick system assessment before going deeper.

### Who Am I Logged In As?

To identify the current user account:

```
whoami
```

This command prints the username of the active session.

This information is critical because permissions depend on the user. Some accounts have administrative privileges, while others are restricted.

Understanding your privilege level determines what actions are possible.

### What Is the Name of This Computer?

Each Windows system has a hostname that identifies it on the network.

To view the computer name:

```
hostname
```

This outputs the system’s short name.

In enterprise environments, hostnames often follow naming conventions that indicate department, location, or system role.

### What Version of Windows Is This?

To obtain detailed system information:

```
systeminfo
```

This command generates extensive output. At a beginner level, focus on key fields:

- **OS Name** – The Windows edition installed
- **OS Version** – The build number
- **System Type** – 32-bit or 64-bit architecture

These details are crucial in vulnerability research, because exploits often target specific Windows builds and architectures.

Knowing the exact version helps determine potential attack paths.

### How Is This Machine Connected to the Network?

To view the network configuration:

```
ipconfig
```

This displays network interface information.

Key elements to observe:

- **IPv4 Address** – The local IP address of the machine
- **Default Gateway** – The router connecting the system to other networks

Understanding network configuration is essential for:

- Network enumeration
- Lateral movement analysis
- Identifying reachable systems

In penetration testing, `ipconfig` is often one of the very first commands executed after gaining access to a machine.

# **Issue met**

During this room, the main difficulty was not the complexity of the commands, but developing a structured approach. It is easy to type commands randomly without understanding why you are using them.

Another challenge was interpreting command outputs correctly. Commands such as `systeminfo` produce a large amount of information, and without knowing what to focus on, it can feel overwhelming.

Understanding the difference between simply executing commands and performing structured enumeration was an important mindset shift.

# **Solution**

The solution was to slow down and think like an analyst instead of just a user.

Rather than memorizing commands, I focused on:

- Understanding what problem each command solves
- Observing the output carefully
- Identifying which pieces of information are useful in a security context

For example:

- `whoami` is not just a username display tool, it tells me my privilege context.
- `ipconfig` is not just networking information, it reveals the machine’s position in the network.
- `systeminfo` is not just system details, it can help identify potential vulnerabilities based on OS version.

By connecting each command to a real-world security objective, the CLI becomes a powerful investigative tool instead of a simple interface.

# **What I have learned**

I learned how to confidently navigate a Windows system using only the Command Prompt.

I can now:

- Identify my current directory and move across the filesystem
- List files, including hidden ones
- Search for specific files across the disk
- Read file contents directly from the terminal
- Identify the current user and privilege context
- Retrieve system version and architecture details
- Analyze basic network configuration

More importantly, I understood that command-line interaction is a core skill in cybersecurity. Many post-exploitation, forensic, and administrative tasks rely entirely on CLI usage.

# **Takeaways**

The Windows CLI is not just an alternative way to use Windows — it is a precision tool.

Graphical interfaces are convenient, but they hide system logic. The command line forces you to understand:

- Where you are
- What you are doing
- What permissions you have
- How the system is structured

This room built the foundation for:

- System enumeration
- Privilege escalation analysis
- Network reconnaissance
- Incident investigation

Mastering the basics now will make advanced techniques easier later.