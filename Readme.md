TITLE: Vagrant Image for setting up the NGINX Proxy for 3Scale APICast 2.0.
=======================================================================

This project allows you to 'hello world' 3Scale using the 'Self-managed Gateway' woth APICast 2.0


Prerequisites
=============
A running Vagrant installation - typically virtualbox + vagrant.
You will also need to have Ansible installed.

 You will need to go onto the 3Scale portal and configure an API to manage.

Installation.
=============
1. There seems to be a nasty bug in ansible (or my plain stupidy :-), but I cannot get the template to accept variables as it should.
Anyway, once you have created an API you will need to config. an access_token which you do from settings.

Now in posession of this, edit the file 

roles >> apicast >> tasks >> main.yml 

and go to the line 

 shell: THREESCALE_PORTAL_ENDPOINT=https://8bb82bcc08163918b205da1257ec3bdbf36b7af4a94a4b8bd890b34983439@redhatmsumner-admin.3scale.net 

 replace the access token with yours...
 and the 3scale domain with yours....

 so it should now be:

 shell: THREESCALE_PORTAL_ENDPOINT=https://<access_token>@<your 3scale domain>

 save and close the file we are now good to go.

2. Having done this then 'vagrant up' from the same directory as the Vagrantfile.


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
