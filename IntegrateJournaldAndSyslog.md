### Setup Rsyslog

- yum install rsyslog
- sudo systemctl enable rsyslog
- sudo systemctl start rsyslog

### Enable Rsyslog to Forward Logs to Journald

- sudo vi /etc/rsyslog.conf

module(load="omjournal")  # Load the omjournal module to send logs to journald

### Configure Log Forwarding

*.*   :omfile:/var/log/syslog   # Send logs to traditional syslog file
*.*   :omjournal:               # Send logs to journald

### Restart rsyslog service

- sudo systemctl restart rsyslog

### Verify Logs in Journald

- journalctl
