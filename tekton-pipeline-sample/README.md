Note : 
- in new version tekton maybe different confoguration need to look up first
Security
========
To give Trigger access to registry(push,update) by running the buildier task we need to give user access to registry permission by adding this command 

<pre>
$ oc policy add-role-to-user registry-editor system:serviceaccount:pipeline-tutorial:pipeline
</pre>
