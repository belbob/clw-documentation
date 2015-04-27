# ssh
Created Tuesday 21 April 2015

<http://docs.opennebula.org/4.6/design_and_installation/building_your_cloud/ignc.html>

### Manual Configuration of Secure Shell Access
You need to create ssh keys for the oneadmin user and configure the host machines so it can connect to them using ssh without need for a password.
Generate oneadmin ssh keys:

``$ ssh-keygen``

When prompted for password press enter so the private key is not encrypted.

Append the public key to ~/.ssh/authorized_keys to let oneadmin user log without the need to type a password.

``$ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys``

Permission requirements for the public key authentication to work:

``$ chmod 700 ~/.ssh/``

``$ chmod 600 ~/.ssh/id_dsa.pub``

``$ chmod 600 ~/.ssh/id_dsa``

``$ chmod 600 ~/.ssh/authorized_keys``

Tell ssh client to not ask before adding hosts to known_hosts file. Also it is a good idea to reduced the connection timeout in case of network problems. This is configured into ~/.ssh/config, see man ssh_config for a complete reference.:

	$ cat ~/.ssh/config
	ConnectTimeout 5
	Host *
	StrictHostKeyChecking no
	UserKnownHostsFile /dev/null

``$ chmod 600 ~/.ssh/config``

Check that the sshd daemon is running in the hosts. Also remove any Banner option from the sshd_config file in the hosts.
Finally, Copy the front-end /var/lib/one/.ssh directory to each one of the hosts in the same path.

Downloading keys

``$ ssh-keyscan CLW UGENT KUL ULB UCL UA > ~/.ssh/known_hosts``

To test your configuration just verify that oneadmin can log in the hosts without being prompt for a password.

### Config sshd
At every node edit [/etc/ssh/sshd_config](file:///etc/ssh/sshd_config)
set:
...
 **PermitEmptyPasswords yes**
...

restart sshd

``# service sshd restart``

Delete password for oneadmin (if needed)

``# passwd -d oneadmin``

