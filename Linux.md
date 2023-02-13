# Linux

- User space sends system calls to the kernel space to get access to the hardware resources.
- To get the OS kernel:

        $uname -r

- To get the kernel messages:

        $dmesg

- To get the recieved events run the following:

        $udevadm monitor

- To get all the prephiral components interconnect:

        $lspci

- To get all the block devices in the pc:

        $lsblk

- To get all the details about the computer CPU:

        $lscpu

- To get all the details about the computer memory:

        # You can add --summary to get the summary.
        $lsmem
        # Also you can use
        $free -m

- To get the entire hardware config:

        $lshw

- To get how long the computer run from last reboot

        $uptime

- To check if a command internal or external use type command:

        $type [command]

- You can go chang directories using following:

        $pushd [directory name]
        $popd [to return back to the previous directory]

- To get more details in the files list:

        $ls -l
        # ordered according the update date
        $ls -lt

- To get help with a command:

        $whatis [command]
        $apropos [command]

- To set an env var. in the terminal:

        $export [var.]=[value]
        # To get all the saved env var.
        $env

- To add a path to the path var.:

        $export PATH=$PATH:[path...]

- To get where a program is stored use:

        $which [program name]

- To compress a file and reduce its size:

        $tar -zcf [file1] [file2] ...

- To get a file that you are searching for:

        $sudo updatedb
        $locate [filename]

- To find a file in a directory:

        $find [directory] -name [fileName]

- You can use -r with grep to search in all files in a directory and -i to neglect the case of the word.

- To forward an error message into a file:

        $..................... 2> [error.txt]
        # The same as | tee

- The TELNET command enables you to log on to a foreign host that supports TCP⁄IP.

        telnet [IP] [port]

- To run an interface:

        $sudo ip link set dev [interfce] up

- To get info about the network connectivity of the host:

        $netstat

- Pluggable authentication module (PAM) in linux is used to authenticate users to programs and services.

- SELinux isolating applications runningon the same system from each other

- Info about users stored in `/etc/passwd` and about groups stored in `/etc/group` and passwords saved in the file `/etc/shadow`

- To get the users those currently logged in:

        $who
        #To get those logged in since last reboot:
        $last

- To get the users that have root previlidge go to `/etc/sudoers`

- To create a new user:

        $useradd [name]
        $passwd [name]
        # Then when the user signed in then to change their password
        $passwd
        #To delete a user
        $userdel [name]
        # To create a group with a specific id
        $groupadd -g [#] [group name]
        # To delete a group
        $groupdel [group name]

- To do not write password every time use password-less ssh:

        1. First generate rsa kies:
        $ssh-keygen -t rsa
        2. Notice that the key will be in `home/[username]/.ssh`
        3. run the following command:
        $ssh-copy-id [username]@servername
        4. Now will ask you to enter the server password and Voila! you can access the server without writing the password every time.
        5. On server the publice key of the user will be saved on `home/[username]/.ssh/authorized_keys`

- To copy files from and to a remote server:

        # -pr for folders
        $scp -pr [from] [to]
        $scp [servername]:[filepath] [path on the host]

- IP tables used to filter the IP addresses that can access the server. To install IP tables:

        $sudo apt install iptables
        # To get the IP tables
        $sudo iptables -L

- To schedual tasks on linux machines use cronjobs:

        # To get all the shedualed tasks
        $crontab -l
        #
        $crontab -e 
        # => this will open editor to add the commands and their schedual.

- To create a fully functional partition:
1. Create the partition
2. Create a filesystem
3. Mount the filesystem to a folder (To make the the changes permanent you need to add the changes to /etc/fstab)

- Logical volume Manager (LVM): it is created of a group of phyical volumes. LVM give the ability to build logical volumes that made of the mixture of all of the physical volumes.

- sed is comand used to replace text in a file or group of files:

        # To exchange "hate" with "love" in a file, Add -i to update the file with the changes
        # You can use any delimeter other than '/' 
        $sed -i 's/hate/love' [filename]
        $sed 's/^hate/love' [filename] > [new file]
- awk is also a tool used to grap specific part of data nd updating files (it uses spaces as a delimeter):

        # To show the first word of each line in a file
        $awk '{print $1}'
        # To get last field in a file 
        $awk '{pint $NF}'
        # To change the delimeter of awk ust -F flag
        $awk -F':' '{print $1, $NF}'
        
- To count the number of outputs: 

        $ ..... --no-headers | wc -l
        

