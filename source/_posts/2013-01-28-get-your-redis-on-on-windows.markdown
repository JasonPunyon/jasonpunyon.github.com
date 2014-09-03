---
layout: post
title: "Get Your Redis &#79;n on Windows"
date: 2013-01-28
comments: true
categories: redis vagrant virtualization windows how-to
---

**TL;DR**: Want a virtual machine running redis in however long it takes you to download 400MB + a little completely automated install time? Follow the instructions [here](https://github.com/JasonPunyon/redishobo) and you'll be on your way.

Well, it only took me a year of having this blog for me to write up something even remotely technical. But here you are, and here I am...so let's just tone down the celebration a little bit and get on with it already.

So...it's hard running a dev environment sometimes. We at the [Stack Exchange](http://www.stackexchange.com) will use anything to get the job done, but on the programmer's side we're mainly a windows shop. One piece of software we've come to know and love is [Redis](http://redis.io) though. We love it so much we've got [antirez](http://antirez.com/latest/0) on speed dial. It's really the greatest. 

Here's where it isn't quite the greatest though (for us): it's really meant to run on Linux. Some people have made mostly working [windows builds](https://github.com/dmajkic/redis/) in the past that were good enough for dev'ing on but had weird behavior when it came to background operations. They're great and I appreciate the work they d(o|id), but they fall behind when redis bumps stable versions (it's behind 1 stable version right now leaving out features like the Lua scripting engine). Microsoft went through the rigamarole of [patching redis](http://oldblog.antirez.com/post/redis-win32-msft-patch.html) so that it will run on windows, but that patch isn't getting merged to master...ever.

So what's a girl to do? When I've been on a team of one and had this kind of problem I thought to myself, "Self! Get VMWare on here, spin up a one off VM with ubuntu and just run it there! Problem Solved!" and many internal high-fives were had. But when you're on a team of 6 (the [Careers](http://careers.stackoverflow.com) team, plug: [we're hiring](http://careers.stackoverflow.com/company/stack-exchange)) that doesn't really scale well. So what are my choices? Let's go to the big board of options:

- Just tell my teammates "Hey, spend a couple hours spinning up your own VM and hope the one you have and the one I have match up and behave exactly the same". (HINT: No)
- Check in a 10 gig VM into source control and push so the other members of the team can run it too? (HINT: NO. That's an example of what we call the "I quit" check-in.)

So how do you solve this problem?

### Enter the Hobo

So it turns out a bunch of other people have this problem too (WEIRD RITE?). A [smart dude](https://twitter.com/mitchellh) decided to solve it and created [Vagrant](http://vagrantup.com). Vagrant is a super simple yet powerful way to create and manage a reproducible virtual dev environment. Check in a couple kB of config and you get a virtual machine (or a multiple machine environment) your whole team can run. Vagrant wraps around [Virtual Box](https://www.virtualbox.org/) for it's virtual machines and it's not just for windows. It runs on Linux and Mac too. Let's run it down.

#### Getting in Installed

Follow the [startup guide](http://docs.vagrantup.com/v1/docs/getting-started/index.html) here. It's basically install VirtualBox and install Vagrant.

#### Creating a machine

To create the machine, the first thing we do is create your Vagrantfile. Don't worry...it isn't a driter fetish. It's just a config file that outlines how your virtual machine is setup. It's also just a bit of ruby. Here's the one we're using:

{% include_code No Drifter Fetishes Here lang:ruby get-your-redis-on-on-windows/Vagrantfile %}

So first we tell Vagrant which box to use. A [box](http://docs.vagrantup.com/v1/docs/getting-started/boxes.html) is essentially a map from a key to a file. Box names can be anything you want, in this case I just have a name telling me that it's ubuntu's latest 64-bit release.

Next we have a url that points to a file. As it says in the comment there, this url points to a box file that will be downloaded if the box with the name in `config.vm.box` doesn't exist. This is nice because it means i send the file and when my teammate runs it it will go fetch everything it needs to create the virtual machine. Brilliant. A bunch of base boxes can be found at [Vagrantbox.es](http://vagrantbox.es). They have many different guest operating systems and versions and such to use. Very cool.

Next we have some port forwarding settings. Vagrant takes care of setting up the network for you, you just have to tell it what you need. So I'm just forwarding to port 6379 on the guest machine (the default port on which redis runs) from port 6379 on my host machine.

Next I [customize](http://docs.vagrantup.com/v1/docs/config/vm/customize.html) the vm to have a gig of memory instead of whatever the base box has by default.

So that's it for the configuration the box. The last line runs a provisioner which will setup the box once it's running. There are a number of provisioners to choose from including Puppet, Chef and shell. This was the gotcha for me when I was doing it the first time. The docs list the provisioners in this order...

{% img center http://i.imgur.com/K8OIqlD.png %}

So I spent an hour or two trying to grok the chef and puppet docs and ended up getting frustrated. Those systems have a bunch of abstractions in them which probably make them great for doing sys-adminny type stuff but in my head I was screaming "AAAARRRGH. JUST LET ME RUN A FUCKING SHELL SCRIPT!". Of course I go back to the vagrant docs after that, look an inch or two down and feel like an idiot. I do wish the bullet points there went in order of increasing complexity though.

Long story short, the provisioner just executes the specified shell script on the guest box after it boots up.

#### Shellack It

So once the machine boots up what do we want it to do? Well, this:

{% include_code get-your-redis-on-on-windows/init.sh %}

So first we make the directories where redis will live. Then we go to the top level one, download and extract the code for the version of redis we're interested in and build it. Then we copy the resulting executables to their final home in `/opt/redis/bin`.

Next we copy an init.d script to where it needs to be, then we copy the redis configuration to where it lives. Add a redis user, start redis and we're all finished.

You might be asking "How did that init.d script and redis configuration get into the vagrant directory on the guest box?"

The way you run vagrant is by going to the directory where the Vagrantfile lives and typing `vagrant up`. That starts the whole ball rolling. When vagrant starts up your VM, it automatically shares the directory where the Vagrantfile is with the guest box at `/vagrant` on the guest box. It's a magical default behavior.

### So that's pretty much it

Well, for now anyway. Vagrant can be used to setup multiple machine environments (which I might do next to test out an elasticsearch cluster for Careers). It has many more bells and whistles to keep your virtual dev environment running lean and mean. I've been super impressed with just how easy it is to work with (total home grown code to get my redis VM up was 31 lines, 15 of which were the shell script for installing redis) and bonus everyone on my team thinks I'm a hero. It's that magical.

+1 Vagrant...+1.

##### Appendix

This is the init.d script I used which I cribbed from [Ian Lewis](http://www.ianlewis.org/en/redis-initd-script).

{% include_code get-your-redis-on-on-windows/redis.init.d %}