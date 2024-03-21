Logging in as the root user
On Local System
Perform the following steps on your local machine.

Run the following command to generate a key.
```bash
$ ssh-keygen
```
or
```
ssh-keygen -t rsa -f ~/.ssh/KEY_FILENAME -C USERNAME -b 2048
```
You’ll be asked to enter passphrase. You can leave that empty.
```
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
```
A pair of keys (public and private) will now be generated and saved at the location you earlier entered.
```
Your identification has been saved in /Users/username/.ssh/id_rsa.
Your public key has been saved in /Users/username/.ssh/id_rsa.pub.
The key fingerprint is:
<Your Key Here> your@email.com
```
Start the ssh-agent:
```
$ eval `ssh-agent`
Agent pid 59566
```
add your key to the agent:
```
$ ssh-add ~/.ssh/id_rsa
```
Copy the contents of ~/.ssh/id_rsa.pub to your clipboard. The file location can vary if you chose a different path for your key earlier.
Hint: It starts with ssh-rsa and ends with something@something.

On your GCP instance
Perform the following steps on your GCP instance.

Connect to your instance by choosing Open in browser window on custom port option.
Open GCP instance in a browser window

Open ~/.ssh/authorized_keys file for editing:
```
$ sudo nano ~/.ssh/authorized_keys
```
Paste the contents of clipboard and press CTRL + S to save the file. Press CTRL + X to exit the editor.

Allow for root login on SSH connections. Open /etc/ssh/sshd_config for editing:
```
$ sudo nano /etc/ssh/sshd_config
```
Uncomment the line starting with PermitRootLogin prohibit-password and add another line right below it as shown:
```
PermitRootLogin prohibit-password
PermitRootLogin yes
```
Note: For systems older thatn Ubuntu 16.04, The lines would look like:
```
PermitRootLogin without-password
PermitRootLogin yes
```
Save the file and exit editor.

Restart SSH service.
```
sudo service ssh reload
```
Connect to GCP from you local machine.
If you followed all the steps correctly, give yourself a pat on the back. You did it!

Now you should be able to ssh to your instance as root user by doing:
```
$ ssh <your root username>@<public IP of your instance>
```
Bonus: If outbound port 22 is blocked on your network
If your ISP has blocked the default SSH port (22), you can use SSH over the HTTPS port (443), which is generally open. Follow the steps given below.

On your GCP instance
Connect to your instance by choosing Open in browser window on custom port option.
Open GCP instance in a browser window

Open /etc/ssh/sshd_config file for editing:
```
$ sudo nano /etc/ssh/sshd_config
```
Uncomment the line
```
#Port 22
```
and replace it with
```
Port 443
```
Save the file and exit the editor.
Restart the SSHD service:
```
sudo service ssh reload
```
Click on the 3 dots next to your instance name and then select View network details option.
View Network Details of a GCP instance

Now click on default-allow-ssh in the list.

selct default_allow_ssh option from the list

Click on Edit option.

Click on Edit

Look for Protocols and ports heading and change it from
```
tcp: 22
```
to
```
tcp:22,443
```
Save the rule.

Connect from you local machine
Now you should be able to ssh to your instance as root user by doing:
```
$ ssh -p 443 <your root username>@<public ip of your instance >
```
That’s it. If you face any problems following the tutorial, comment down below and I might be able to help out.

Cheers!
