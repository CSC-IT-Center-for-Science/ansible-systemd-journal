[![Build Status](https://travis-ci.com/CSCfi/ansible-systemd-journal.svg?branch=master)](https://travis-ci.com/CSCfi/ansible-systemd-journal)

systemd-journal
========

An [ansible role](https://galaxy.ansible.com/cscfi/systemd-journal) to configure
the systemd [journal](https://www.freedesktop.org/software/systemd/man/systemd-journald.service.html)
via [journald.conf](https://www.freedesktop.org/software/systemd/man/journald.conf.html)
to handle all logging and more tightly limit it's log storage, suitable for an
SSD on a laptop.


    ---
    - hosts: localhost

      roles:
        - cscfi.systemd-journal

Requirements
------------

Any systemd distribution.

Role Variables
--------------

    vars:
      - systemd_journal_create_directory: True
      - systemd_journal_rsyslog_package_state: present
      - systemd_journal_storage: auto
      - systemd_journal_compress: yes
      - systemd_journal_system_max_use: 500M
      - systemd_journal_system_max_file_size: 50M
      - systemd_journal_max_retention_sec: 0
      - systemd_journal_restart_state: started

The journal will be default use as much free space as it can get and then delete
old logs if other data fills a disk. That's not very friendly for SSD drives so
`systemd_journal_system_max_use` limits this to 500M by defaut, or whatever you
customise it to.

If you want to set custom options not directly supported by this role you can also overwrite systemd_journal_configuration directly.

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
