# Bash Scripts for System Administration

Collection of Bash scripts designed to enhance system administration and user management tasks on Unix-like systems. These scripts aim to automate various aspects of system monitoring, user behavior analysis, and resource utilization management. Each script serves a specific purpose, contributing to the overall efficiency and security of the system environment.

The scripts included address common operational needs such as identifying and managing user activity, monitoring network usage, assessing disk space utilization, and analyzing login patterns. They offer flexible options to gather information on users, their processes, and their file activities, providing administrators with valuable insights for maintaining system integrity and performance.

## Table of Contents

- [bad_user.sh](#bad_usersh)
- [class_act.sh](#class_actsh)
- [info_user.sh](#info_usersh)
- [net_out.sh](#net_outsh)
- [ocupation.sh](#ocupationsh)
- [user_stats.sh](#user_statssh)

## Scripts

### bad_user.sh

**Description:**  
This script identifies and manages users with improper behavior on the system. It can be used to detect users with no files and no processes running, or users who have not logged in or modified any files in a specified time interval.

**Usage:**  
```console
./bad_user.sh [-p]  
./bad_user.sh [-t] [d/m]
```

- `-p`: Lists users with no files and no processes running.
- `-t [d/m]`: Lists users with no active processes, no logins, and no file modifications in the specified time interval (days or months).

**Additional Notes:**  
This script requires administrator privileges to run correctly.

**Details:**  
- The script reads the password file to get a list of all users.
- For each user, it checks if their home directory exists and counts the number of files they own.
- If a user has no files:
  - With the `-p` option, it checks if the user has no running processes.
  - With the `-t` option, it checks if the user has no active processes, no logins, and no file modifications within the specified time interval.
- Users meeting these conditions are listed.

### class_act.sh

**Description:**  
This script generates a report on the files modified by a specific user within a given time period.

**Usage:**  
```console
./class_act.sh [Number_of_days] [Name+surname]
```

- `Number_of_days`: The number of days to look back for modified files.
- `Name+surname`: The full name of the user (as stored in the system).

**Additional Notes:**  
The script extracts information from the `/etc/passwd` file to determine the user's home directory and name. It then finds all files in the user's home directory that have been modified within the specified number of days, counts these files, and calculates the total space they occupy in MegaBytes.

**Details:**  
- Validates that exactly two arguments are provided: the number of days and the user's name.
- Retrieves the user's information from the `/etc/passwd` file.
- Lists files modified in the user's home directory within the given time frame.
- Counts the number of modified files and sums their sizes.
- Converts the total file size from Bytes to MegaBytes and prints the result.

### infouser.sh

**Description:**  
This script provides detailed information about a specific user on the system, including their home directory, its size, other directories they own files in, and the number of active processes.

**Usage:**  
```console
./infouser.sh [username]
```

- `username`: The name of the user for whom the information is being retrieved.

**Additional Notes:**  
The script extracts the user's home directory and calculates its size. It also finds other directories where the user owns files and counts the number of active processes.

**Details:**  
- Validates that exactly one argument (username) is provided.
- Retrieves the user's information from the `/etc/passwd` file.
- Calculates the size of the user's home directory using the `du` command.
- Identifies top-level directories where the user owns files.
- Counts the number of active processes for the user.
- Prints the gathered information in a readable format.

### net-out.sh

**Description:**  
This script displays network interface statistics, including the name of each interface and the number of transmitted packets.

**Usage:**  
```console
./net-out.sh [time_in_sec]
```

- `time_in_sec`: Optional. Specifies the interval (in seconds) to refresh and display network statistics.

**Additional Notes:**  
The script uses `ifconfig` to gather information about network interfaces and the number of transmitted packets. It can be run without parameters to show current network statistics once, or with a parameter to continuously update and display statistics at the specified interval.

**Details:**  
- Configures the `PATH` environment variable to include `/sbin` for access to system utilities like `ifconfig`.
- Defines a function `net` to retrieve and display network interface names and packet counts.
- Supports an optional parameter to specify the refresh interval for displaying network statistics.
- Provides informative usage messages if incorrect parameters are provided.

### ocupation.sh

**Description:**  
The `ocupation.sh` script monitors disk space usage for individual users or users within specified groups, checking if their disk usage exceeds a defined limit.

**Usage:**  
```console
./ocupation.sh [-g group] max_space_allowed(G/M/K/B)
```

- `max_space_allowed`: Specifies the maximum allowed disk space per user or group, with units like G (Gigabytes), M (Megabytes), K (Kilobytes), or B (Bytes).
- `-g group`: Optional. Checks disk space usage for users within the specified group.

**Additional Notes:**  
The script calculates disk space usage based on home directories defined in `/etc/passwd` and optionally groups from `/etc/group`. It alerts users who exceed the specified disk space limit by modifying their `.profile` to display a warning message.

**Details:**  
- Validates command-line options and formats.
- Retrieves and calculates disk space usage for individual users or group members.
- Converts disk space limits into Bytes for accurate comparison.
- Outputs users or group members who exceed the specified disk space limit and their respective disk usage.

### user_stats.sh

**Description:**  
The `user_stats.sh` script provides summaries of user login activity and currently running processes on a Unix-like system.

**Usage:**  
```console
./user_stats.sh
```

**Additional Notes:**
- Ensure the script is executed with appropriate permissions to access user information (`/etc/passwd`).
- The script uses standard Unix utilities (`last`, `ps`, `top`) to gather information, which may vary slightly in output format across different Unix-like systems.

**Details:**

- The script iterates through all users defined in `/etc/passwd`.
- For each user, it calculates the total login time and number of logins using the `last` command.
- Displays the total login time in minutes and the number of logins for users who have logged in at least once.
- Similar to the login summary, it iterates through all users defined in `/etc/passwd`.
- Counts the number of processes each user currently has running using `ps`.
- If the user has active processes, it calculates the total CPU usage percentage based on the processes' CPU utilization reported by `top`.
- Displays the number of processes and total CPU usage percentage for users with active processes.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.