TITLE: Vagrant Image for setting up the NGINX Proxy for 3Scale APICast 2.0.
=======================================================================

This project allows you to 'hello world' 3Scale using the 'Self-managed Gateway' with APICast 3.0
This was done specifically for the build of the OAuth2 / OpenIdConnect integration with RH-SSO.


Prerequisites
=============
A running Vagrant installation - typically virtualbox + vagrant.
You will also need to have Ansible installed.

You will need to go onto the 3Scale portal and configure an API to manage.

Installation.
=============

Build your API in the 3Scale management portal.
When you have done this then open the file ./vars/defaults.yml

Edit the 3 lines of code appropriately for your environment.

#######################################
#### VARS You WILL have to customise
#######################################

RHSSO: "https://myrhsso.mydomain.com/auth/realms/rhsso"
GATEWAY_URI: "@somedomain-admin.3scale.net"
GATEWAY_TOKEN: "ec255f933asl3302lsf075b8e1c767ea9206ac885ef744f657bc3c279a"

The redhat SSO may not be necessary depending on whether or not you wish to have Oauth2 working.


Once this is done then type 'vagrant up' - vagrant will spin up your new gateway vm and then provision it using the ansible playbooks being bootstrapped.


Description.
=============
What does this do?

1. Sets up a vagrant Centos VM. on top of your host OS.
2. Sets up and then installs openresty (the distribution of nginx used by 3scale).
3. Checks out the version of apicast you specified under vars/defaults.yml  - apicast-3.0.0 being the default.
4. Finally sets up a start script for you.

Could you use this playbook to set up a nginx production server? - more or less.
Maybe this will be a following appdev practise project :-)

How does this differ from the apicast version 1.0?
==================================================
You do not pull and replace the config. from the portal, rather the gateway is now config. to pull this code directly.
The conf. has been changed so that any custom changes do not get blatted by subsequent changes to the gateway.

Usage
=========
I suggest that you open four terminals and cd each of them to be the root of the gateway project ie. where the Vagrantfile lives.

1. - you will use this for running vagrant - up / halt / destroy / provision

2. - you will use this for stop/starting the gateway : type 'vagrant ssh', then 'cd /l/binary'

      ./gateway.sh start or stop

    you might need to be root for this... sudo su  

3. - use this one for tailing the logs : type 'vagrant ssh', then 'cd /l/gateway/apicast-3.0.0/apicast/logs'

  tail -f error.log

4. - use this one for general noodling about... : vagrant ssh, then cd /gateway


5. - Your gateway will now be available on:

    http://192.168.33.10:8080/your stuff.




One last thing....
===================
Make certain that you are calling proxy via the same entered value in the 'public api' section of the portal.

So in otherwords the value that you type to call this proxy might look something like this:

curl -v -X GET "http://192.168.33.10:8080/search?term=beyonce&entity=musicVideo&user_key=7b86fdea742e29594449ab12f0b487c0"

NOTE:

Your user_key is not the access_token used previously, so in the above connection string we are using the user_key,
this allows the gateway to auth. against the portal.


The 'http://192.168.33.10' must appear in your 'Public Base URL' in the api definition within the portal.

Finally
========
Enjoy!


Vagranting...

from the directory containing the Vagrantfile :

vagrant up - starts the vm (and provision)
vagrant provision - run the playbook on the vm.
vagrant destroy - flatten and start again.
vagrant ssh - takes you into the vm.
-- vagrant will map the root of your project (where the vagrantfile is located ) to the /vagrant
