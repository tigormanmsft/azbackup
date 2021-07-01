# azbackup
Useful scripts for diagnosing issues with Azure VM Backup for app-consistent Oracle database backups

# bash script "azbackup_verify.sh"

This script is intended for use in validating and troubleshooting the configuration of an Azure Linux VM for app-consistent backups of Oracle databases using Azure VM Backup.

### Prerequisites

Full "sudo" permissions are *required* by this script.

This script uses the "sudo" privilege escalation command, and so it should be run under the administrative Linux OS account for this Azure VM (which is granted full "sudo" permissions at VM creation), or else run under the Linux root OS account.

### Validations performed

    1. existence and contents of the "/etc/azure/workload.conf" config file
       a. validate "workload_type" specified in config file is "oracle"
       b. validate "configuration_path" specified is a file with entries in correct format
       c. validate "timeout" specified in config file between 0 and 3600 seconds
       d. validate Linux OS account specified as "linux_user" in config file
       e. validate Linux OS group assigned to "linux_user" is Oracle SYSBACKUP group
    2. existence of Oracle "pre-script" and "post-script"
       a. within root-protected "/var/lib/waagent" subdirectory
    3. For each Oracle database instance listed in the file referenced by "configuration_path"...
       a. validate existence of "$ORACLE_HOME" directory and specific subdirectories
       b. validate existence of "config.c" source file
       c. validate that the defined Linux OS group for OS authentication of the SYSBACKUP role is the primary OS group of the Linux OS account (i.e. "linux_user" attribute)
       d. validate that it is possible to connect to Oracle SQL*Plus under the Linux OS account for Azure VM Backup with the SYSBACKUP role
       e. validate that the AZMESSAGE stored procedure exists and is VALID

### Command-line Parameters
    (none)
       
### Return status
    0 - success
    1 - failure, please refer to error messages emitted by the script
