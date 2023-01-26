# Jenkins

_Note: To add a public key to Jenkins to eanble jenkins cli on your terminal, you have to paste it in the right format, for example:_

        ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDTkkUH0Gz1L9VPQ8iEif7kOloqI6sJuRkQgEXQF4LpAk9jrA/7u4ifKhA7sQemnfTg9QzL6wFH9wN/dj2B19VUQKh4ElSEbG4dJHFNh8JbnMDiSHaNmi0bq81Sqi++hFuhtG3pcDqYbLtSLqBkL8hL/+v3cQhjyOvwKPQaFMEh2v0Rv37W1l8AsT3LxfB0SZjFgYOgzB23U7wd89BIvAuado1QTkBqmSF9xctqHU+8A3j0wk8Qv6Gz573idXp7IQtmcQ5Ol7OBVS+ki13eJhrhIBYuGps0GGTR+tNXyJvSXqnRjB/DBYJbBvbSsMirzC9cOodQxO0NkBCduh/bsuDoHBIyMlHjJtEsTyLaf3DRdCWq6UqDe9AitoIbnusukM743aduJmJLjdyyPI6MudT+nrwZh1KkfSt7w72Pet2Trx5EUrdyjDswnRf44zHlKaEVBpLHMHG0K+lmq2m0VzWYgNdFqfZXaieDiREH5AaC4qBeJDt+D2qTnYFvACPcApk= mike@jenkins-server_

_Plugins allow you to connect to other services._

- Promethmeus and Grafana are used to folow the logs of jenkins server, Grafana used specifically to visualize the logs of the jenkins server.

## Pipeline

As you know jenkins server is a tool used to grap the code from a hub and contanerize, build(execute) and then deploy in the test env. then to the prod. env.. The pipeline is the file that contains the workflow of the CI/CD process.

The build agent is the server that tackle the processing burden, it may be the jenkins server itself but that is not recommended. Built agent also can be a Docker Container. To add a build agent you have to add the host admin in the "Manage Credentials" and then through "Manage Nodes" you can add the build agent.

- The main structure of the jenkins file is the folllowing:

  ```
  pipeline
  agent any *"agent{label'host_name'}"*
   tools {
      // Install the Maven version configured as "M3" and add it to the path.
      maven "M3"
  }

  stages {
      stage('Build') {
          steps {
              git"http:///"
          }
      }
  }
  ```

- To create an image through jenkins file you have to install the following plugins: - Docker - Docker pipeline
  then use the follwing command to create the image:
  ```
  steps{
    script{
        app = docker.build("adminturneddevops/go-webapp-sample")
    }
  }
  ```

## Some important Bash Shell commands:

- su - (userID) => To switch the user of the computer or server ex: _$ su - mahdi_
- weget [url]=> is a command-line utility for downloading files from the web ex: _weget http//..._
- ssh -i /home/mike/.ssh/jenkins_key -l mike -p 8022 jenkins-server help =>
  -i flag is used to point to mike's private SSH key. Remember, we have already added the public key in the Jenkins configuration.
  -l is the login user which in our example is mike
  -p is the port which we found out in the previous step to be 8022
- To get info about the OS that you are using through bash shell you can use the following command:

        $cat /etc/os-release
        # Or with wildcards
        $cat /etc/*release*

- To run a file with .sh extention:

        $sh fileName.sh
        #To run it contiuously
        $ ./fielname.sh

- To create nested folders in one command:

        $ mkdir -p folder1/../..

- To get info about the user:

        $ id

- To switch the user on the host:

        $ su [the user want to switch to]

- To start a service on the host:

        # systemctl start [service name]
        $ systemctl start httpd
        # To stop the service
        $ systemctl stop httpd
        # to get the status
        $ systemctl status httpd
        # To configure a service to run whenever the host bootup
        $ systemctl enable httpd
        # To disable a service
        $ systemctl disable httpd

- To create a service for an app then go to `etc/systemd/system`, then create a file (.service) with name of the app them fill the file with running instruction in the following structure:

        [Unit]
        Descreption=Running watermelon program

        [Service]
        ExecStart= The running commands
        ExecStartPre=  running needed dependencies
        ExecStartPost=

        # To restart the app whenever it fails
        Restat= always

        # To enable running whenever the system bootup,
        # and make using enable command possible
        [Install]
        WantedBy= multi-user.target

Then, run the following command and the service will be ready to start:

        $systemctl daemon-reload

- To copy a directory from a place to another, use the following command:

        $cp -r [directory name] [intended location path]

- To get the structure of the file as a tree:

                $cd [folder path]; tree

- To get the location of a pakage:

                # [] => Example
                $[pip] show [flask]
                # The more useful command
                $[python3] -c "import sys; print(sys.path)"

- To get the network interfaces on the host:

        $ip link

- To get the addresses assigned to all interfaces:

        $ip addr
        # To set an IP address on an interface
        $ ip addr add [IP addresss] [interface]

- To get the routing table on your host:

        $ip route
        # To add entries to the routing table
        $ip route add [IP] dev [inerface]

- To check if forwarding requests is enabled on your device or not(works as router) if the value is 1 means forwards and 0 is the opposite:

        $cat proc/sys/net/ipv4/ip_forward

- To add an entry to the local DNS on you host add the IP and the name of the other host into `etc/hosts` => 192.0.0.1 db

- To get the DNS server that your host connected to:

        $cat etc/resolv.config
        # You can add more name servers and to make searching about easier for
        some websites add: [search  [the domain name]] => now the top DN will
        take you to this site.

- The computer at first stage looks at the local DN file then to the DNS, to change this precedence update `etc/nsswitch.config`

- To compile java code and create the .class files run the following command:

                $javac [source code file name (.java)]

- To run java program run:

                $java [class name without any extensions]

- JAR (java archive) file contains multiple compressed java files. Creating a jar file implicate creating a manifest file that contains info about where the program start running. To create a jar file use the following command:

                $jar [Names of the files included in the jar just space between them]
                # To run JAR files:
                $ java -jar [the jar file]

- Use javadoc to format your app docs:

                $javadoc -d doc [file name]

- WAR (web archive) contains java files and static files as HTML and CSS.
- Storage driver on most Linux distribution is AUFS.
- To install all packages of a python program, write them in a txt file then run the following command:

                $pip install -r [requirements.txt]

- Python has pakages other than pip like 'easy_install' ans 'wheels'
- To query data from jason or yaml file you can use what called jasonPath, to see the syntax of jasonPath visit the following page: `https://support.smartbear.com/alertsite/docs/monitors/api/endpoint/jsonpath.html`
