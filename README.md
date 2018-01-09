########################  Apache Configuration  ######################

- Requires Ansible 1.9.3 or newer
- Expects an AMI with ubuntu 12.04 & pre-installed Apache/2.2.4 (Unix) & Configuration flaws

Configuration Flaws :

- User/Group : The name (or #number) of the user/group to run httpd as.
  It is usually good practice to create a dedicated user and group for
  running httpd, as with most system services. Ex: apache as an user and a group
  Current User and Group are root user.

- Status of httpd service : This is giving error beacuse of "The program 'lynx' is currently not installed. "

- The program 'lynx' is not installed.

- Proxy pass configuration for 'server-status' is missing in httpd.conf.
  server-status request should be handled by apache itself rather than the node.js application


This playbook deploy a simple configuration of apache and restart the apache service
to make sure new configuration has been loaded.

Upload Configuration Files :

- httpd.conf  (proxypass configuration for 'server-status' has been placed)

- httpd-vhosts.conf

Note: Developers can edit these both files and ansible playbook will automatically upload the changes and
      make sure that the changes has been applied and apache service (httpd) has been restarted.
      Developers can add more backend application in the httpd-vhosts.conf or
      edit  the current node.js application proxy configuration.

Then run the playbook, like this:

    Usage: ansible-playbook -i hosts site.yml --private-key=PRIVATE_KEY -u user

	Run playbook : ansible-playbook -i hosts site.yml --private-key=~/Downloads/iw-tech-test-ali-fener/techtest-ali-fener.pem -u ubuntu

The playbook will install 'lynx' program and upload configuration files and restart the apache service.

Playbook testing:

Need to have the AMI of current server to use to create more AWS instances and run the playbook against all of them
to approve playbook configuration & idempotency.

### Automation Scenarios

- Remote host configuration

    * Run ansible playbook from any machine
    * Make sure that the remote machine can access to apache server via ssh
    * Specify apache server host configuration
    * Many apache server can be configured at the same time with same configuration

- Local host configuration

    * Install ansible with user-data or use an AMI with pre-installed ansible
    * Use AWS S3 to store configuration files, use SSE KMS Encryption at rest
    * Copy all the needed configuration files from AWS S3 to the new server (use an IAM role which can access to the S3 bucket)
    * Run ansible playbook, use control machine as local (ansible_connection=local)
    * A new apache server will be configured automatically
    * Terraform, Cloudformation or AutoScaling Launch Configuration can be used to launch these instances automatically

