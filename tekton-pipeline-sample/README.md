Security
========
To give Trigger access to registry(push,update) by running the buildier task we need to give user access to registry permission by adding this command 

<pre>
$ oc policy add-role-to-user registry-viewer <user_name>
$ oc policy add-role-to-user registry-editor <user_name>
</pre>