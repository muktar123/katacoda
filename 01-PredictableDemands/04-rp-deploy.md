
Now lets create a Deployment for a Pod with resource limits set.

Check the Deployment that we have prepared with

`bat deployment.yml`{{execute}}

As you can see in the `resources:` section you find `requests:` and `limist:` specifications for starting up the container with initial 100 MB (request) up to 200 MB (limits) memory allocated. We are also using three replicas of our service.

This works nicely for our Spring Boost example REST app, so we can create that with

`kubectl create -f deployment.yml`{{execute}}

We can again verify that the Pods are running with

`watch kubectl get pods`{{execute}}

Let's wait until all Pods are ready indicated by a `1/1` in the `READY` column. This means that the Pods pass the readiness check and can be used by a Service.

We create now a Service of type `nodePort`, which opens up a port 31666 on every node. In our case this is of course only the master node.

`kubectl create -f service.yml`{{execute interrupt}}

The service can be accessed in Katacoda with

`curl -s http://[[HOST_IP]]:31666/info | jq .`{{execute}}

or you can also reach it externally via http://[[HOST_SUBDOMAIN]]-31666-[[KATACODA_HOST]].environments.katacoda.com/info

The [/actuator/health](http://[[HOST_SUBDOMAIN]]-31666-[[KATACODA_HOST]].environments.katacoda.com//actuator/health) endpoint exposes also the memory used by the JVM from the POV of the JVM. Can you spot how much memory the random-generator is using ?

Try

`curl -s http://[[HOST_IP]]:31666/actuator/health | jq .`{{execute}}

to examine that health endpoint.

Everythings looks good now. But what happens if we change the restrictions so that the JVM will blow that away ?

Let's check that in the next step.
