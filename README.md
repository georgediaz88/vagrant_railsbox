Having fun installing all that software needed to start a new rails project? No? Then use this VM instead.

# Get up and running

## 1. Install VirtualBox
https://www.virtualbox.org/

## 2. Install vagrant and librarian
``
  $ bundle
``

## 3. Install chef cookbooks
``
  $ librarian-chef install
``

## 4. Build the VM
``
  $ vagrant up
``

# What you'll get
* ruby -  v2.0
* rails - latest
* git
* postgresql
* sqlite
* mysql
* nodejs
* nginx web server + passenger

# Usage

There's a folder called 'homegrown_apps' in this directory, it will be mounted into the VM at "/vagrant/homegrown_apps". This is where the rails app should be cloned. Since this folder is shared between the two environments you can edit code from your host OS and run rails commands (etc) from within the VM.

To login to the VM run:

``
  $ vagrant ssh
``

When you are done, exit the VM and run the following command to suspend it until later.

``
  $ vagrant suspend
``

Resume work at anytime:

``
  $ vagrant up
``