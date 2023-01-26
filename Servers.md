# Servers

This chapter will discuss some aspects of some common used servers, and some tips about them.

## Apache Web Server

- Mostly used to host static appplications (HTML/ CSS/ JS)
- To get the user access logs:

        $cat /var/log/httpd/access_log

- To get the errors logs:

        $cat /var/log/httpd/error_log

- The configuration file of Apache is `etc/httpd/conf/httpd.conf`

## Apache Tomcat

Used to host java based applications

## General Notes

To make an app available on all the host interfaces (each has different IP address) then define the host as 0.0.0.0:

        >> app.run(port=8000, host='0.0.0.0')
