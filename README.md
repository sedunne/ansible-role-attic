# Ansible Attic/Borg Role

Ansible role that can install the attic or borg backup software. It also can manage the SSH key for the backup user, a basic backup script, and cron job for the backup task.

### Disclaimer

This role isn't really actively used or maintained. It was created when I was testing Attic/Borg, and thought it might be useful to someone else, so I'm making it available. Because of this, it may assume certain things, or not function in a very portable way, as it was intended to be applied for a specific use-case.

### Requirements

  * RHEL-based system w/yum
  * Ansible 2.0 (though it may work with older versions; only tested w/Ansible 2.0+)
  * attic or borg package present in enabled repository

### Variables

Variables below given in 'var_name: default' format:

  * `attic_package_name: attic` - name of the package to install from repository
  * `attic_script_path: '/usr/local/sbin/atticbackup.sh'` - path to the backup script provided by the role
  * `attic_log_path: '/var/log/attic.log'` - path for the logfile for the backup script
  * `attic_cache_directory: '~/.cache/attic'` - directory to use for the client-side cache
  * `attic_bin_name: attic` - the binary name on the client server to use for commands (mostly here for when using w/Borg)
  * `attic_manage_package: True` - whether or not to manage the package its self (can be used if attic/borg installed through another role/playbook)
  * `attic_package_state: True` - when managing package, the state to use
  * `attic_manage_ssh_key: False` - whether or not to manage the ssh key for the "backup user" (used for authentication with remote host)
  * `attic_is_borg: False` - whether or not Attic is actually Borg
  * `attic_init_command: 'init'` - the command to use when initializing a backup repository
  * `attic_create_command: 'create --stats'` - the command to use when creating a backup
  * `attic_prune_command: 'prune -v'` - the command to use when pruning backups
  * `attic_backup_user: root` - the user the backups run under
  * `attic_server_user: attic` - the user to connect as on the 'remote' host
  * `attic_server_name: '127.0.0.1'` - the 'remote' host to connect to
  * `attic_backup_path: '/home/'` - the path to backup
  * `attic_days_keep: 30` - how many "days" (backups) to keep
  * `attic_cron_user: root` - the user to run the cron under
  * `attic_cron_minute: 0` - the minute to run the cron at
  * `attic_cron_hour: 0` - the hour to run the cron at
  * `attic_cron_day: '*'` - the day to run the cron at
  * `attic_cron_month: '*'` - the month to run the cron at
  * `attic_cron_weekday: '*'` - the day of week to run the cron at

### Example

```
---
- hosts: attic
  vars:
    attic_package_name: 'http://repo.example.com/attic.rpm'
    attic_script_path: '/root/atticbackup.sh'
    attic_server_name: '192.168.1.69'
    attic_backup_path: '/home/'
    attic_cache_directory: '/attic/cache'
    attic_cron_minute: 30
    attic_cron_hour: 23 
    attic_manage_ssh_key: True
    attic_log_path: '/var/log/attic.log'

  roles:
    - { role: attic, become: yes }
```

In the above example we have `attic_manage_ssh_key` set to `True`, which causes the role to use a variable named `attic_backup_user_private_key` to be loaded. By default this variable is not defined. Since this is sensitive information, use of Ansible Vault is highly recomended when managing the SSH private key through this role.

### License

This role is released under the MIT license. See LICENSE file for copyright, and full details.
