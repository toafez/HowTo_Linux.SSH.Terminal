English | [Deutsch](README.md)

# HowTo: SSH key in the Linux terminal
[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fgithub.com%2Ftoafez%2FHowTo_Linux.SSH.Terminal&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)

## What is this about?
The following instructions describe **how to set up SSH public key authentication** from a local Linux system to a remote Linux server **using a terminal program**.

## Introduction
Secure Shell, or SSH for short, is a network protocol for establishing encrypted connections between devices on the local network or over the Internet. SSH public key authentication uses a private and public key pair to establish a password-free login to a remote server, which can be further secured by entering an additional passphrase if required. The use of such a key pair is therefore much more difficult to compromise than entering a password.


#### _Note: Text in uppercase within square brackets is a placeholder and must be replaced with your own information, but may be used in some places for information purposes only. Please note that the square brackets are part of the placeholder and must also be removed when replacing with your own information.

## Open a terminal
First you will need a terminal program of your choice to access the console of your client operating system.

1. After starting the terminal program, you should be in your own user home directory. You can check this by typing **pwd** _(stands for 'print working directory')_ and pressing enter.

    ```
    [USERNAME]@[CLIENT-PC]:~$ pwd
    /home/[USERNAME]

    ```

    Following this example, you are in the home directory of the user [USERNAME] (correctly, of course, this should be your username), which is exactly where you should be. Depending on the Linux operating system you are using, a different path may be displayed here. For example, if you are using a Synology NAS, the path to the home directory of the currently logged in user would be `/var/services/homes/[USERNAME]`.

    **Note:** For better readability, all the following entries are displayed without using the `[USERNAME]@[CLIENT-PC]:~$` prompt.

## Create SSH directory structure

1. First, create a new hidden directory called **.ssh** in the home directory of the currently logged in user.

    `mkdir .ssh`

## Create an SSH key pair
1. You can now start creating a new **SSH key pair** consisting of a **public** and a **private key**. This is done using the **ssh-keygen** command. By simply typing the ssh-keygen command, different encryption algorithms can be used depending on the configuration. You should therefore always specify which algorithm is to be used. In the following, the **RSA** algorithm is used with an encryption of **4096** bits.

    `ssh-keygen -t rsa -b 4096`

    (**-t** stands for type, i.e. the type of encryption, in this case rsa for RSA protocol 2)_.

    (**-b** stands for bit, i.e. the bit length of the encryption key, in this case 4096 bits)_ 1.

2. Immediately after running the command, you will be asked for the filename to store the SSH keys in. If you want to keep the suggested path and filename, simply press Enter to confirm. Otherwise, you can enter an alternative filename without extension using the full path. In this example, the suggested filename is used.

	```
	Generating public/private rsa key pair.
    Enter file in which to save the key (/home/[USERNAME]/.ssh/id_rsa):
    Created directory '/home/[USERNAME]/.ssh'.
	```

3. You will then be asked to enter a passphrase, i.e. a password. Using a passphrase is highly recommended as it prevents the private key from being easily copied and used, even if your client operating system has been compromised. The disadvantage of a passphrase is that you have to enter it every time you want to connect via SSH. This can be counterproductive if you later want to establish unattended and thus automated SSH connections, for example to perform automated backups. In this case, we strongly advise against entering a passphrase. It is up to you whether you want to use a passphrase or not. In this example, no passphrase is used, but the following two prompts are simply skipped by pressing Enter.

	```
	Enter passphrase (empty for no passphrase):
	Enter same passphrase again:
	```

	A similar issue to this one will follow...

	```
	Your identification has been saved in /home/[USERNAME]/.ssh/id_rsa
    Your public key has been saved in /home/[USERNAME]/.ssh/id_rsa.pub
    The key fingerprint is:
	SHA256:ScteBU5B3/m7/cod5OqlH8JSDpiNC8oGGYx0sEhoHFM [USERNAME]@[CLIENT-PC]
	The key's randomart image is:
	+---[RSA 4096]----+
	|o*+E     .=.     |
	|=o*      o o . . |
	|oo o    . . o o  |
	|    o  o o=.   . |
	|   o   .S+.o . ..|
	|    o ..... = o .|
	|     +  .. . + * |
	|    .       ..=.=|
	|            .+++=|
	+----[SHA256]-----+
	```

4. After execution, there will be a file called **id_rsa** containing the **private** key and a file called **id_rsa.pub** containing the **public** key in the directory `/home/[USERNAME]/.ssh/`.

5. Change to this directory with **cd** _(stands for 'change directory')_.

    `cd .ssh`

6. The prompt at this point should include the .ssh directory as confirmation and look like this

    `[USERNAME]@[CLIENT-PC]:~/ssh$`

7. The contents of the directory can be displayed with the command **ls** _(stands for 'list')_, followed by further options if necessary.

    `ls -la`

    (option **l** = file information is output in long form)_.

    (option **a** = also show hidden files and folders starting with a dot)_.

    **Example of output**

    ```
    [USERNAME]@[CLIENT-PC]:~/ssh$ ls -la
    total 16
    drwxrwxr-x 2 [USERNAME] [USERNAME] 4096 Dez 31 07:00 .
    drwxr-x--- 8 [USERNAME] [USERNAME] 4096 Dez 31 07:00 ..
    -rw------- 1 [USERNAME] [USERNAME] 3389 Dez 31 07:00 id_rsa
    -rw-r--r-- 1 [USERNAME] [USERNAME]  746 Dez 31 07:00 id_rsa.pub
    ```

    **Note:** You can display the contents of the directory you are currently in at any time. This can be used in conjunction with other options to show more or less information, or to improve the readability of the output.

8. If required, you can display the contents of the public key on the console with the command **cat** _(stands for 'concatenate', i.e. link file contents)_. If you want to give the public key to other people, you can also copy the key and save it in a separate text file.

    `cat id_rsa.pub`

    **Example of output**
    ```
    ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCbTJMv9N+pU8n7SaDa9RLazZwB+cR0ke2ggq/Xbria3fsuLGG/wrd5mOt1KYSg7FWiiHUSec0s1sX0OAbWk8qX47UaJKe7SP6so39kVjkRGINACsojvYI773LBpDuKmGzBqsTpBSlYLGLtA7rs669YSEV+8M6Z7NJj6YySDRBL/TSx/cjiJyh5h6yHwW/+nqzFBteqzVoBQ241nGdVzcb5S16DnJLhn2MB7es70yOzq02y5XpFSPswme6sH5/jIkNhp1OU2++jPxLNOkcN73WB3pc3jz7bZxkBSh7zJxy1o39kVjkRGISbznxxbIGDn20iAFoLF6IHl0mWhiuYVwSRjZ8M1Kf71qYv7X3RHMrKQQM5qYtpk/R0NdGrvemDP/5gAI0wOG1D/RQD8nBHzHwuNvLGLtA7rs66tcfj1g+jwwMPzEgX5MsisbIKfhwLJZE34zyfWDj/Vl5n44yG7/YDkYmxtPRsYbfLUm5D+q5dhm5+g3H8t0/eQXHel7chM5I8j4pElDhFOCF8jeaCeawxAGrRn6Dab93BHF1wwowOp4kVWKw0lnvTQfbcIc5BSrxR2uYfQk9dwiiDeE6npEN11mTezRN/tsZ6JE4OrPL8oGha7Fa73AIMvyo+/kKsl2wMpBHYRZq70dp0MTUfIAQPAvNM31yNY4B8oIKV+wBxXw== [USERNAME]@[CLIENT-PC]
    ```

## Creating the authorised_keys file and adding your own public key
1. The **authorised_keys** file will later contain all the public keys of known remote servers that wish to access your client operating system to enable a passwordless SSH connection, in addition to your own key. The following command adds your own public key to authorized_keys. If the file does not exist, it will be created automatically.

    `cat id_rsa.pub >> ~/.ssh/authorised_keys`

    (Command **>** file = (overwrite) standard output of the command in the target file.)_.

    (Command **>>** file = append the standard output of the command to the target file.)_.

    **Note:** The **tilde** character _(**~**)_ in the Linux file system represents the current user's home directory. This is a shortcut that allows users to access their own home directory without having to enter the full path. The path `~/.ssh/authorised_keys` is therefore identical to the path `/home/[USERNAME]/.ssh/authorised_keys`.

## Adjusting folder and file permissions
It is important that both the .ssh directory and the files in it have certain permissions to restrict unauthorised access and increase security. The command **chmod** _(stands for 'change mode')_ changes the permissions of folders and files, which are as follows.

`chmod 700 ~/.ssh`

`chmod 600 ~/.ssh/id_rsa*`

`chmod 600 ~/.ssh/authorized_keys`

## Making an SSH connection to your remote server
1. To connect to your remote server, you need to know the IP address and port, as well as the user name and password. To do this, replace the placeholders for [PORT], [USERNAME] and [IP ADDRESS] in the following command with your own data. Then run the following command

    `ssh -p [PORT] [USERNAME]@[IP-ADRESS]`


2. After confirming the following command with the Enter key, you should see a message at the end of the text asking you to confirm the connection with **yes**. Are you sure you want to continue the connection (yes/no/[fingerprint])?` This message only appears if you are connecting to the remote server via SSH for the first time. A **fingerprint** is stored in the `~/.ssh/known_hosts` file on your client operating system to allow future connections.

    ```
    The authenticity of host ‘[IP-ADRESS] ([IP-ADRESS])’ can't be established.
    ED25519 key fingerprint is SHA256:DosguIv2tIYP+9n3BIrSM2df1841CdXbIc4pAZ01ehA.
    This key is not known by any other names.
    Are you sure you want to continue connecting (yes/no/[fingerprint])?
    ```

    You will now be asked to enter your password to log in to your remote server.

    `[USERNAME]@[IP ADDRESS]'s password:`

3. Once you have successfully logged in, you should be on the console of your remote server. Again, check that you are in the logged-in user's home directory.

    ```
    [USERNAME]@[REMOTE-SERVER]:~$ pwd
    /home/[USERNAME]

    ```

4. Create a new hidden directory called .ssh and give it the necessary permissions.

    `mkdir .ssh`

    `chmod 700 .ssh`

6. Log out of the remote server to return to your client OS console.

    `exit`

## Transferring the Public Key to the Remote Server
1. The contents of the id_rsa.pub file, and thus the public key, can now be transferred to the remote server via an SSH connection to the authorised_keys file, which can be created if it does not yet exist. To do this, replace the placeholders for [PORT], [USERNAME] and [IP ADDRESS] in the following command with your own without using the square brackets. Then run the following command

    `cat ~/.ssh/id_rsa.pub | ssh -p [PORT] [USERNAME]@[IP-ADRESS] "cat >> ~/.ssh/authorised_keys"`

2. After successfully logging in with your password, the public key was saved to the ~/.ssh/authorised_keys file on your remote server and the connection was closed. You are still on the console of your local Linux operating system.


## To re-establish the SSH connection to your remote server...
1. You should now be able to log in to the remote server without having to enter a password. To check if this works, log in to the remote server again.

    `ssh -p [PORT] [USERNAME]@[IP-ADRESS]`

2. If all went well, you should now be on your remote server's console without having entered a password (unless you are using one of the passphrases mentioned above). And while you're at it, you can also correct the permissions on the authorized_keys file by issuing the following command

    `chmod 600 ~/.ssh/authorised_keys`

3. You have now successfully set up passwordless SSH key authentication.
