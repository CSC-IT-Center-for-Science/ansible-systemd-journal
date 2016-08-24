systemd-journal
========

An [ansible role](https://galaxy.ansibleworks.com/list#/roles/221) to configure
the systemd [journal](http://0pointer.de/blog/projects/systemctl-journal.html)
to handle all logging and more tightly limit it's log storage, suitable for an
SSD on a laptop.

This role will remove the rsyslog package on F19 (already the default on F20)
unless you tell it otherwise.

    ---
    - hosts: localhost

      roles:
        - groks.systemd-journal

Requirements
------------

A recent version of [Fedora](https://fedoraproject.org/get-fedora) Linux.

Role Variables
--------------

    vars:
      - systemd_journal_rsyslog_package_state: present
      - systemd_journal_system_max_use: 500M
      - systemd_journal_system_max_file_size: 50M
      - systemd_journal_restart_state: started

The journal will be default use as much free space as it can get and then delete
old logs if other data fills a disk. That's not very friendly for SSD drives so
`systemd_journal_system_max_use` limits this to 500M by defaut, or whatever you
customise it to.

The `systemd_journal_rsyslog_package_state` variable can be `absent` or
`present` and if absent (not the default) the rsyslog package will be removed.

The `systemd_journal_restart_state` variable is `started` - because in many cases the restart failed on CentOS 7 even though the service was seemingly restarted.

<pre>
Aug 24 09:47:02 hostname systemd-journald[22636]: Failed to set file attributes: Operation not supported
Aug 24 09:47:02 hostname systemd-journald[22636]: Failed to create new runtime journal: No such file or directory
Aug 24 09:47:02 hostname systemd-journald[22636]: Failed to set file attributes: Operation not supported
Aug 24 09:47:02 hostname systemd-journald[22636]: Failed to set file attributes: Operation not supported
[snip]
Aug 24 09:47:02 hostname systemd-journald[22636]: Assertion 'f' failed at src/journal/journal-file.c:132, function journal_file_close(). Aborting.
Aug 24 09:47:02 hostname systemd-journal[22638]: Journal started

</pre>

Dependencies
------------

None.

License
-------

BSD
