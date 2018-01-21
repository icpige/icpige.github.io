---
title: Docker in CloudCannon
category: Operations
author: tim
image:
image_featured: false
permalink: /operations/2014/12/17/docker-in-cloudcannon-developers.html
---

Environments can be tricky. Making sure the libraries are the same between your developer machines and production servers, updating dependencies consistently and onboarding new staff, it can turn into a mess. Luckily there are tools that can help us. I have previously used [Chef](https://www.chef.io/chef/) for this type of problem but now [Docker](https://www.docker.com/) has arrived to make things easier.

<img src="/images/blog/operations/dockerlogo.png" alt="The docker logo">

There are [a lot](https://www.docker.com/whatisdocker/) of [in depth](http://www.zdnet.com/article/what-is-docker-and-why-is-it-so-darn-popular/) [introductions to docker](http://developerblog.redhat.com/2014/05/15/practical-introduction-to-docker-containers/), but basically it is a box that holds your app and its dependencies. You can then run this on any machine and get a consistent experience. Today I will be focusing on how we use docker on our development machines. If you are thinking of trying docker then this is where you will start out, so hopefully you will find some useful tips from our experience.

### Out of Linux

Docker uses linux containers [(LXC)](https://linuxcontainers.org/) which naturally requires a linux based operating system. So what do you do with your cool coworkers and their Mac laptops? You can use [boot2docker](https://github.com/boot2docker/boot2docker) which sets up a virtual machine behind the scenes so that you can use docker on Windows or OS X. To simplify running our services we made a bash script which names the container based on its folder. We had issues connecting to amazon services from boot2docker due to timeouts. This could be solved by syncing the time with the ntp server, which the script runs each time it starts boot2docker.


{% highlight bash %}
#!/bin/bash

path=${PWD##*/}

#Get a name for our docker service
NAME=$(echo $path | tr -d ' '| tr '[:upper:]' '[:lower:]')

#See what boot2docker is currently up to
STATUS=$(boot2docker status)

#Check which ports are required
ports=$(sed -n '/^EXPOSE/p' dockerfile | cut -d ' ' -f 2- )

OPEN=''

for i in $ports; do
 OPEN+=" -p $i:$i "
done

#Turn on boot2docker if it is off
if [[ 'poweroff' == $STATUS ]]; then
  boot2docker start
  settime
fi

#Sync the time so we can connect to servers
function settime {
  boot2docker ssh "sudo ntpclient -s -h 0.nz.pool.ntp.org"
}

#Build the new docker image
docker build --tag="cloudcannon/$NAME" .

#Remove the current container if there is one
docker stop $NAME
docker rm $NAME

#Start the new container
docker run -d -e "environment=development" -e "USER=$USER" $OPEN --name $NAME cloudcannon/$NAME
{% endhighlight %}

### The base image

Docker images are created using a Dockerfile which has instructions on all the dependencies and requirements for your application. There are a couple of camps about how you should use these dockerfiles, either [running a single process per container](https://docs.docker.com/articles/dockerfile_best-practices/) or [treating it as a light weight vm](http://phusion.github.io/baseimage-docker/). We have a foot in each camp, with our node services only running a single process and our ruby a bit more fleshed out.

For node we have our own base cloudcannon image which installs node, npm and the common libraries. The individual services start with this then add the package.json and call npm, add their application files, expose any required ports then start the application as the CMD. An important part of docker is that it caches on each instruction, so each build will reuse the cache until it gets to an instruction that has changed. Doing the more expensive and less likely to change package.json dependencies first means that they can be cached so it won’t rebuild it each time you change the application files. This setup works well for our node services which are each focused on performing one action, so it is great to have them as lightweight as possible.

Our ruby on rails app is based on the [phusion base image](https://github.com/phusion/passenger-docker) . This gives a solid base to work from with rails and nginx already installed. After installing our libraries we follow the same pattern as the node apps by adding the gemfile, doing the bundle install then adding the application files. Changing to this order gave a significant speedup in our iterations, changing from taking about 10 minutes to do the install each time to seconds when it is only code changes.

### Wrapping up

A final tip is it is especially important with your docker images is to be strict with your version numbers. Whether it is the version of the base image or the dependencies in your gem file you want to specify exactly what is going into it. This will not only allow you to be consistent across your environments but also ensure that you are reusing the cache until you expect it to change.

Hopefully that gave you an insight into what it will be like starting out in docker. Many of the tutorials you find show you the base hello world for your language, but going the step beyond that to making your specific application work with it can be a larger step than it seems, so make sure you set aside some time to experiment. Next time we will be looking at how we deploy and what docker looks like server side. In the mean time you can try out [this handy interactive docker tutorial](https://www.docker.com/tryit/?utm_source=rainforestqa&utm_medium=link&utm_campaign=deployment-academy)
