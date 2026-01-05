This is what I expect to talk about and how. So you can read my slides and pair it with this to have an idea.

### Title Slide
Hello everyone! I’ll be talking about building a cloud service provider today.

### Details about me
So this slide is a bit about me and what I do, but let’s jump over those details.

### Picture of DevCon from a smaller room
This is a picture from home, the biggest annual tech conference, and it struck me just how small everything is compared to Europe and your events here. You see, I come from Mauritius, and if you don’t know where that is, it’s in the middle of the Indian Ocean.

### Picture of Google Maps showing Mauritius

### Picture of my workshop at DevCon
This story originates from a workshop I tried running on Kubernetes, where everyone would have a cluster of their own and not interfere with each other. It needed to be on the cloud so that we don’t spend longer fighting WSL or different Linux flavours, just for it to work. 

### Picture of data center distribution
Another thing we don’t really have, down in the southern hemisphere, is cloud service providers. Here’s a map of the DCs of the main CSPs, and you’ll notice that the southern hemisphere is pretty barren. For us, our closest CSPs, save for the handful of local ones, are in South Africa.

So, how do I build a cloud?

### Requirements for a cloud
The thing about clouds is that you are going to have many users, users who do not trust each other. So, you need all the users to be separated. I imagine the two ways of running different users’ workloads in isolation would be either virtual machines or containers.
VMs are out of the question for their resource usage and overhead. But how do you run containers easily and isolated?
Then, obviously, it’s the cloud so has to live on someone else’s computer.
And lastly, I would prefer it to be self-managed, as I do not want to provision VMs manually. 

### Virtual Clusters
Everyone has heard of KinD, Kubernetes in Docker. It’s ideal to experiment with and learn on a local device like laptop. But building on top of that, you have virtual clusters which can be run inside an existing kubernetes cluster - kubernetes in kubernetes.
There are two options out there - VCluster from Loft Labs, and K3k from SUSE.

### K3k and VCluster
These two work by provisioning virtual clusters on a host cluster, and that virtual cluster exists as pods, with the API server exposed through a service. You even have the option to passthrough host resources such as storage and ingress classes, as well as configmaps and secrets.
Virtual Clusters are then isolated through namespaces, network policies, and such.
You can also apply resource quotas, pod security admission level, priority class, and network policies from the host, to make managing the virtual clusters easier.

### KRaft, with reasoning behind the name
Introducing KRaft, which is built on top of K3k, and uses Traefik and Longhorn. Those two aren’t strict requirements but they are what I have.

### How It’s Built
KRaft is a set of a few services which run on a host cluster. So, you have an auth service for everything user management, login, registration, and so on.
Then a cluster service to manage cluster provisioning, exposing the API, retrieving the Kubeconfig file, and deleting clusters.
A small resource service to keep an eye on cluster metrics
Frontend of html served through an nginx pod
MariaDB for all the data, and it’s less resource hungry than running MySQL or CNPG.

### Resource page
And this is the resource usage of the running pods. I emphasised on writing it in Rust, so it is pretty lightweight, which fits my target of being able to run this anywhere.

### Slides showing off the KRaft UI

### Safe Workloads
Obviously, one of the major considerations and concerns I had making a cloud platform is that I’m handling people’s data, so it needs to be secure. For starters, isolation is helped by users having their own API server and not interacting with others. At the cluster level, namespaces, network policies, and pod security admission level are used for isolation. And lastly, each cluster comes with its own coredns deployment.

### Jana’s messages
These are some commentary from my first tester who deployed something on the virtual cluster, and was impressed.

### Work in progress?
Not sure whether to include the slide or include but not talk about it. 5min isn’t a lot.


