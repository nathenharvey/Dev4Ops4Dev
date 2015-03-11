# Setting Up Your Development Environment

We want all attendees of this workshop to develop in the same environment.  To do this, we will be using a combination of VirtualBox and Vagrant so that each attendee (no matter what OS their laptop is running on) will be able to develop from an Ubuntu Linux 14.04 virtual machine.  We are assuming some familiarity with the linux command line, but if you are not familiar please reach out to an instructor and your fellow students.

## What you will need to install on your laptop

VirtualBox
Vagrant

## Introduction to VirtualBox and Vagrant

[VirtualBox](https://www.virtualbox.org/) allows your system to run multiple virtual machines from your laptop's OS without needing to reboot or affect your laptop's OS.

[Vagrant](https://www.vagrantup.com/) is a wrapper for VirtualBox VMs which allows you to create portable work environments you can take from system to system.  It makes it easy to ssh into a virtual machine and interact with it the way you would interact with a cloud server.

## Setting up VirtualBox

Download the VirtualBox package appropriate for your Operating system [here](https://www.virtualbox.org/wiki/Downloads)
After it is downloaded and installed, follow the instructions [here](http://www.virtualbox.org/manual/ch01.html#idp91929072) to start up VirtualBox.

[TO DO: Decide if we want to provide Virtual Box installation packages on a USB stick to avoid hitting the wifi so hard]

## Setting up Vagrant

Download the Vagrant package appropriate for your Operating System [here](http://www.vagrantup.com/downloads)

[TO DO: Decide if we want to provide Vagrant installation packages on a USB stick to avoid hitting the wifi so hard]

### OS X

Follow the Vagrant setup instructions located [here](https://docs.vagrantup.com/v2/getting-started/index.html)

Create a new directory to host your workshop material.  Call it whatever you like.

Open up a terminal window and change directories into your workshop directory.  IE, if you created your workshop folder within your documents directory, you would navigate there by typing this in the terminal.

```bash
  $ cd Documents/my_workshop_folder
```

Head on down to the "Using Vagrant" section of these instructions.

### Windows 7

You must also install PuTTY (a Telnet and SSH client) and PuTTY gen (an RSA and DSA key generation utility).  You can downlad and install these [here](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)

Create a new folder to host your workshop material.  Call it whatever you like, I created mine on my windows desktop but you can create it wherever you choose.

Hold down shift and right click on the project folder, then select "open command window here"

If you receive the error "Timed out while waiting for the machine to boot", follow these instructions:

1) Open up the VagrantFile in your project directory with Notepad (it's easiest do do this OUTSIDE of the command prompt)
2) Add these lines in between "Vagrant.configure(2) do |config|" and "end" lines

  ```bash
   config.vm.provider :virtualbox do |vb|
     vb.gui = true
   end
  ```
3) Save the file
4) Navigate back to your command prompt
5) Destroy the previous Vagrant virtual machine with this command

  ```bash
    $ vagrant destroy
  ```
6) The start the

Source: this [Stack Overflow Answer](http://stackoverflow.com/a/22575302).

Head on down to the "Using Vagrant" section of these instructions.

### Windows 8.1

### Linux

## Starting Vagrant

Let's create a Vagrant box that runs Ubuntu 14.04 LTS (Trusty Tahr)

In your project directory (using terminal if you're on a Mac or Linux box, using the command prompt if you're on a Windows box) type in this command:

```bash
  $ vagrant init ubuntu/trusty64
```
This will create a Vagrant file in the project directory, which will allow us to configure our Vagrant box.  Go ahead and open it up and take a look through if you like.

[TO DO: See if there's a way to do this locally]

Then run this command to start up your new Vagrant box

```bash
  $ vagrant up
```

## SSHing into a Vagrant Box

Now it's time to SSH into your new Vagrant box so we can use it as a development environment!  The procedure for doing this varies slightly by Operating System, so please follow the appropriate instructions below.

### OS X or Linux

In your project folder (make sure it's the same directory with the Vagrant file you just created), run this command:

```bash
  $ vagrant ssh
```

This will log you into your Vagrant box as root.

### Windows 7 or 8.1

## Updating your system

[TO DO: Capture in a workstation setup Chef recipe?]

The first thing we want to do is update the operating system on our Vagrant box.  To do this in Ubuntu 14.04, run:

```bash
  $ sudo apt-get update
```

## Setting up Chef

[TO DO: Capture in a workstation setup Chef recipe?]

We'll be using Chef throughout this workshop to set up a webserver, so let's get the ChefDK (Chef Developer Kit) installed on your Vagrant box.

Run this command:

```bash
  $ curl -L https://www.chef.io/chef/install.sh | sudo bash -s -- -P chefdk
```

This will take a few minutes.  Once it is complete (you'll see a message that says "Thank you for installing Chef Development Kit!"), verify that ChefDK installed like this:

```bash
  $ chef --version
```

You should see something along the lines of "Chef Development Kit Version: 0.4.0"

## Configuring Ruby

[TO DO: Capture in a workstation setup Chef recipe?]

We'll be using Ruby to create Chef code.  Although ChefDK does come with a current version of Ruby, there is also an earlier version of Ruby already installed on our Ubuntu 14.04 system.

We need to configure are our development environment to use the ChefDK version of Ruby, rather than the system version of Ruby.  To do this, first run:

```bash
  $ which ruby
```

If it returns something like:

```bash
  usr/bin/ruby
```

That means your development environment is using the system ruby, rather than the ChefDK version of Ruby.  To fix this, run:

```bash
  $ echo 'eval "$(chef shell-init bash)"' >> ~/.bash_profile
```

This adds a line to your .bash_profile file, telling it to use the ChefDK version of Ruby.

Now we need to apply this change.  To do this, run:

```bash
  $ source ~/.bash_profile
```

Then re-run:

```bash
  $ which ruby
```

It should now return:

```bash
  /opt/chefdk/embedded/bin/ruby
```

There are some additional packages you will need to install to work with Ruby on Ubuntu.  You can install these with this command:

[TO DO: Capture in workstation set up Chef recipe?]

```bash
  $ sudo apt-get install git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev python-software-properties
```


## Setting up Git

[TO DO: Capture in a workstation setup Chef recipe?]

Next, we will be using Git for source control.

[TO DO: Explain source control here?]

To install Git, run this command:

```bash
  $ sudo apt-get install git
```

### Connecting to Github

[TO DO: Explain Github]

If you have not already done so, create a [Github account](https://github.com/).  This will give us a place outside of our developer workstation to keep our Chef recipes, etc.

Now let's connect your developer workstation to your Github account.  To do this, first set up your global git name (this is what identifies you in your commits) through this command:

```bash
  $ git config --global user.name "Your Name"
```

Now configure your global git email address (this will be included in your commits) through this command:

```bash
  $ git config --global user.email "your_email@your_email_domain.com"
```

Check that the values are stored correctly by running:

```bash
  $ git config --list
```

Next, you'll need to create an SSH key on your developer workstation and add it to your Github accout to allow you to pull and push repositories from/to Github.  Github provides [excellent instructions](https://help.github.com/articles/generating-ssh-keys/) on how to do this.  If you need help, please ask!

## Setting up AWS

### Updating AWS Gems

Later in this workshop we'll be deploying to live instances on AWS.  ChefDK includes some gems to assist with this, but let's update them to ensure we have the latest versions.

```bash
  $ chef gem update chef-provisioning chef-provisioning-aws
```

If you recieve this warning when updating these gems:

```bash
  WARNING:  You don't have /home/vagrant/.chefdk/gem/ruby/2.1.0/bin in your PATH,
            gem executables will not run.
```

Run this command:

[TO DO: Explain what this is doing]

```bash
  $ PATH=$HOME/.chefdk/gem/ruby/2.1.0/bin:/opt/chefdk/bin:$PATH
```

You don't need to update the gems again, but go ahead if you'd like to confirm the warning does not appear again.

### Setting up your AWS config

In order to deploy to AWS, we need to provide our credentials in a place they can be easily accessed.  To do this, first create a directory called ".aws" to hold these credentials.

```bash
  $ mkdir -p ~/.aws
```

Then create a config file in this directory:

```bash
  $ touch ~/.aws/config
```

Then open the file using your preferred text editor (I use VIM)

[TO DO: Briefly explain text editor options on Ubuntu?  Vim, nano, etc?]

```bash
  $ vim ~/.aws/config
```

Add in the following values:

[TO DO: Figure out how users will get AWS credentials - Chef will provide?]

(ChefConf takes place on the west coast, so we will be using the us-west-2 region)

```bash
  region=us-west-2
  aws_access_key_id = your_aws_access_key_id
  aws_secret_access_key = your_aws_secret_access_key
```

Then save and close the file.

## Conclusion

And that sets up your development environment for this workshop!  Now onto using it!