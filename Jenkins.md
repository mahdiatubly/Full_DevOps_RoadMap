## Some important Bash Shell commands:

- su - (userID) => To switch the user of the computer or server ex: _$ su - mahdi_
- weget [url]=> is a command-line utility for downloading files from the web ex: _weget http//..._
- ssh -i /home/mike/.ssh/jenkins_key -l mike -p 8022 jenkins-server help =>
  -i flag is used to point to mike's private SSH key. Remember, we have already added the public key in the Jenkins configuration.
  -l is the login user which in our example is mike
  -p is the port which we found out in the previous step to be 8022
- To get info about the OS that you are using through bash shell you can use the following command:

        cat /etc/os-release

_Note: To add publick key to Jenkins to eanble jenkins cli on your terminal, you have to paste it in the right format, for example: ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDTkkUH0Gz1L9VPQ8iEif7kOloqI6sJuRkQgEXQF4LpAk9jrA/7u4ifKhA7sQemnfTg9QzL6wFH9wN/dj2B19VUQKh4ElSEbG4dJHFNh8JbnMDiSHaNmi0bq81Sqi++hFuhtG3pcDqYbLtSLqBkL8hL/+v3cQhjyOvwKPQaFMEh2v0Rv37W1l8AsT3LxfB0SZjFgYOgzB23U7wd89BIvAuado1QTkBqmSF9xctqHU+8A3j0wk8Qv6Gz573idXp7IQtmcQ5Ol7OBVS+ki13eJhrhIBYuGps0GGTR+tNXyJvSXqnRjB/DBYJbBvbSsMirzC9cOodQxO0NkBCduh/bsuDoHBIyMlHjJtEsTyLaf3DRdCWq6UqDe9AitoIbnusukM743aduJmJLjdyyPI6MudT+nrwZh1KkfSt7w72Pet2Trx5EUrdyjDswnRf44zHlKaEVBpLHMHG0K+lmq2m0VzWYgNdFqfZXaieDiREH5AaC4qBeJDt+D2qTnYFvACPcApk= mike@jenkins-server_

_Plugins allow you to connect to other services._

---

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
