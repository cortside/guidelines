# Cloning WebApiStarter project

should create new repo that is clone of WebApiStarter project
should work with devops to get build happening
should work with devops to get deploy happening
should work with devops to get database created for all environments
should add new api to healthmonitor
should add new api to status site
should add new apiresource to ids
should add new client for cloned-api in ids (for talking to policyserver)
should add new policy to policyserver

add scope to some known client
make sure that the widget endpoints work
  * should be able to call template endpoint succesfully with authentication and permissions

** make sure it all works!

committed newman collection that client credentials could be plugged into

ef5 healthcheck 

this template has stuff that you will want to remove or rename after you figure out which pieces of it make sense for the service being done
i.e. not all will consume messages or may not publish messages, have custom healthchecks, etc

 
how to setup a new client
* secret for non-production
* handling of secret for production
how to setup a new apiresource

what is the difference between apiresource and client

example restclient
