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
3. Puts in a copy of the APICast project, compiles the LUA and pulls your config. from the portal.
3. Deploys the apicast API from a fixed version - you can find this under the roles/apicast/files dir.
4. finally starts apicast

Could you use this playbook to set up a nginx production server? - more or less.
Maybe this will be a following appdev practise project :-)

How does this differ from the apicast version 1.0?
==================================================
You do not pull and replace the config. from the portal, rather the gateway is now config. to pull this code directly.
The conf. has been changed so that any custom changes do not get blatted by subsequent changes to the gateway.

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
