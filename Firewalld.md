### Use firewalld and NAT(On VM 2)

Start and enable firewalld:

- systemctl start firewalld
- systemctl enable firewalld

Add NAT rule:

- firewall-cmd --add-masquerade --permanent
- firewall-cmd --reload

Configure firewall rules:

- sudo firewall-cmd --list-all # list all firewall rules
- sudo firewall-cmd --get-default-zone # Get Default Zone

Allow SSH and HTTP Traffic from VM1

- sudo firewall-cmd --add-service=ssh --permanent
- sudo firewall-cmd --add-service=http --permanent
- sudo firewall-cmd --zone=public --add-source=VM1-IP --permanent

# Reload firewall to apply changes
- sudo firewall-cmd --reload

# Test firewall rules (On VM 1)

- ssh user@vm1-ip
- curl vm1-ip
