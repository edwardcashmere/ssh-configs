# SSH COMMANDS
Generate the ssh-key run the command in your terminal
then either rename your key or type enter to continue with the generic given name which is id_rsa  for private key and id_rsa.pub for public key

```
ssh-keygen -t rsa

```

add Identity

```
ssh-add /home/user/.ssh/id_rsa

```

## Add the Indentity to Github
___

```
cat /home/user/.ssh/id_rsa.pub

```

If the ssh key is meant for github, go to settings look for ssh keys and gpg keys and paste the copied keys after running the cat command here.Then you are done.You can now use the key to access your github.

## Server Key
___
 
If the key was meant be a server key then ssh into your server using your root password and run the following commands in root.

```
mkdir /home/user/.ssh

touch /home/user/authorized_keys

sudo nano /home/user/authorized_keys
```

Run the following command on your local machine:

```
cat /home/user/.ssh/id_rsa.pub
```
This will give you the public which you copy and paste into the authorized_keys file after the sudo nano command opens the file.ctr + x ,you will be prompted if you want to save the file type 'Y' for yes then enter to save.Now exit the server by typing exit the ssh into the server as shown below:
```bash
ssh user@134.565.56.31


```
Replace the user with your username and the ip addresswith your ip and it should automatically ssh you in into your server without needing the password .

## Disable Root Password Login For the Server

Type the following commands in your bash terminal to open the file

```bash
sudo nano /etc/ssh/sshd_config

```
After which naviagate to the section similar to as below and change the yes to no.

```bash
#LoginGraceTime 2m
PermitRootLogin yes
#StrictModes yes
#MaxAuthTries 6
#MaxAuthSessions 10
```
After changing the option to no CTR + x  a prompt of whether you want to save the new file will appear type 'Y' then enter to save.

Reload the saved file by typing the command below and

```bash
sudo systemctl reload sshd

```
then your are done no one can ssh using the root password to your server . This is mainly to protect against brute force attacks into your server.

# Adding a SSH-KEY into your server 
You will mainly do this when you want to be able access other commands like git cloning or pull from Github or GitLab without being propmted for the password each time.
First confirm that the user created in your server owns the ssh file.run
```bash 
ls -la
```
then look for the ssh file 
 
 
 *edwin edwin   807 Sep  7 11:09 .profile 
 2 root  root    .ssh*

 As you can see my file is owned by the root ,hence if I try to generate any ssh key I will get permission denied error. I have to change ownership from root to the user I created .I do this by running the commnands below in the terminal.

 ```bash
 sudo chown -R edwin:edwin /home/edwin
 ```

 If you run **ls -la** again you will notice the ssh file switched ownership from root to the your current user. i.e mine is now *edwin edwin .ssh*

After this is done you can run in the terminal:
```bash
ssh-keygen -t rsa

```
You will get propmted with a file path path incase you want to rename the ssh file type if you dont need this ,otherwise type the same path.i.e /home/*user*/.ssh/id_rsa_github.
Then type enter and enter a passphrase in, this is just for some extra security or you can skip this step by just pressing enter again until you see the weird figure below.
The key's randomart image is:
___
+---[RSA 3072]----+
|        ..+      |
|        .*.o.    |
|        OoE+.o   |
|      o*=B==*    |
|      =+SBo*     |
|     o *+oo      |
|    o + .o       |
|   . o =. .      |
|    .   +o       |
+----[SHA256]-----+
___

run:
```bash
eval `ssh-agent -s`

```
then:

```bash

ssh-add /home/*user*/.ssh/id_rsa_github
```
This will add the Identity the cat the id_rsa_github.pub or whatever you named your public key by running the command below .

```bash
cat /home/*user*/.ssh/id_rsa_github.pub

```
copy this whatever is inside the file and paste it again to github.
and now you are done.







