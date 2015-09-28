# Introducing OpenCartVM

OpenCartVM is a Vagrant virtual machine for rapidly testing and developing OpenCart and extensions.  Built on Ubuntu 14.04 LTS x64, OpenCartVM allows you to deploy local OpenCart environments in minutes.  The automated setup will:

  * Install OpenCart
  * Install vQmod
  * Enable SEO URLs
  * Setup the ocMod extension installer (OpenCart 2.x)

# Features

  * PHP 5.5.9
  * Composer
  * Xdebug

# Requirements

  * [VirtualBox](https://www.virtualbox.org)
  * [Vagrant](https://www.vagrantup.com/)

# Installation

Install with a few simple commands.

Clone OpenCartVM to a local project.

```
git clone https://github.com/mainspringdev/opencartvm-210x.git project-name
```

Switch to the project directory.

```
cd project-name
```

Start the virtual machine.

```
vagrant up
```

If it's the first time running OpenCartVM it may take a few minutes for Vagrant to download the ``mainspringdev/opencart`` virtual machine.

Once the vagrant up command is completed the OpenCart site can be accessed at http://192.168.21.00.  And that's all there is to it!

# Credentials

The following usernames and passwords are used in OpenCartVM.

### SSH
**username:** vagrant  
**password:** vagrant

### OpenCart Administration
**username:** admin  
**password:** admin

### FTP
**hostname:** 192.168.21.00  
**username:** vagrant  
**password:** vagrant

### Database
**database:** opencart  
**username:** root  
**password:** root

# Database Access

phpMyAdmin was not included to decrease the size of OpenCartVM.  One of the SQL clients below can be used for connecting to and modifying the database.

  * [HeidiSQL](http://www.heidisql.com/) Windows (free)
  * [Sequel Pro](http://www.sequelpro.com/) Mac OS X (free)
  * [Navicat](http://www.navicat.com/) Windows, Mac OS X, Linux (commercial)

## HeidiSQL

The following setup will connect you to the database using HeidiSQL.

### Settings tab
  - Under ```Network type``` select ```MySQL (SSH tunnel)```.
  - Under ```Hostname / IP``` enter ```127.0.0.1```.
  - Under ```User``` enter ```root```.
  - Under ```Password``` enter ```root```.
  - Under ```Databases``` enter ```opencart``` or leave blank.

### SSH Tunnel tab
  - Under ```plink.exe location``` select the path to plink.  If you don't have plink it can be downloaded individually or as part of the putty installer at http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html.
  - Under ```SSH host + port``` enter the IP ```192.168.21.00```.
  - Under ```Username``` enter ```vagrant```.
  - Under ```Password``` enter ```vagrant```.
   
Press the ```Open``` button.  When prompted to store the key in cache select ```Yes```.

## Sequel Pro

The following setup should connect you to the database using Sequel Pro (untested).

### SSH tab
  - Under ```MySQL Host``` enter ```127.0.0.1```.
  - Under ```username``` enter ```root```.
  - Under ```Password``` enter ```root```.
  - Under ```Database``` enter ```opencart``` or leave blank.
  - Under ```SSH Host``` enter ```192.168.21.00```.
  - Under ```SSH User``` enter ```vagrant```.
  - Under ```SSH Password``` enter ```vagrant```.
 
Press the ```Connect``` button.

# Customization

The OpenCartVM is set up so customizations can easily be made in the Vagrantfile.  The top of each file Vagrantfile contains settings similar to the following:

```ruby
$ip = "192.168.21.00"
$idekey = "PHPSTORM"
$admin_user = "admin"
$admin_password = "admin"
$admin_email = "admin@example.com"
```

### IP address ($ip)

The default IP address for an OpenCartVM will begin with 192.168.  The third octet will be made from the OpenCart major and minor version and the fourth octet will be made up of the feature and patch version.  For example, OpenCart 1.5.6.4 is 192.168.15.64, OpenCart 2.0.0.0 is 192.168.20.00, and OpenCart 2.0.3.1 is 192.168.20.31.  The one exception is the OpenCartVM connected to the GitHub dev/master branch which is 192.168.222.222.

### IDE Key ($idekey)

Used for remote debugging with Xdebug. See http://xdebug.org/docs/remote for more information.

### Admin Settings ($admin_user, $admin_password, $admin_email)

These include the OpenCart administrator username, password, and email address for use in the store backend.

### Provisioning

Each OpenCartVM Vagrantfile contains a shell provisioning script which can be modified.

```ruby
  config.vm.provision "shell", inline: <<-SHELL
    # ...
  SHELL
```

Any Linux commands will be executed during an initial ```vagrant up```.  For more information see https://docs.vagrantup.com/v2/provisioning/shell.html.  *Make changes with caution.  Modification to the provisioning script may cause it to fail.*

# OpenCartVM As Extension Base

OpenCartVM allows you to keep a fully deployable environment with your extension version control.

Copy the contents of OpenCartVM as the base of your extension or fork it using git.

```
git clone https://github.com/mainspringdev/opencartvm-210x.git project-name
cd project-name
git remote rm origin
git remote add origin <extension_repo>
git push -u origin master
```

The deployed files for your extension should go in the ``upload`` directory.  To make sure they're properly added by version control edit the ``.gitignore`` file and add ``!/upload/path/to/file.php`` for each of the extension files in upload.  For example:

```
/.idea
/.vagrant
/nbproject
/upload/*
!/upload/.gitkeep
!/upload/vqmod/xml/blog.xml
!/upload/admin/controller/information/blog.php
!/upload/admin/language/english/information/blog.php
!/upload/admin/model/information/blog.php
!/upload/admin/view/information/blog.php
!/upload/catalog/controller/information/blog.php
!/upload/catalog/language/english/information/blog.php
!/upload/catalog/model/information/blog.php
!/upload/catalog/view/information/blog.php
```

Only the ``/upload`` directory and ``Vagrantfile`` are required for OpenCartVM to function.  All other files may be safely removed.
