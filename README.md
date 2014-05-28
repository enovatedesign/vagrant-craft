Vagrant Craft
=============

A puppet provisioned Vagrant development server for Craft CMS projects. It uses the standard Vagrant Ubuntu Precise base box `precise64.box`.

**Please note: This is intended for development purposes only.**

What do you get?
----------------

The latest versions of:

* Nginx (ppa:nginx/stable)
* PHP-FPM (ppa:ondrej/php5)
* phpMyAdmin
* MySQL

**Tested on Vagrant 1.6.2.**

Usage
=====

First you need to install [Virtualbox](https://www.virtualbox.org/) and [Vagrant](http://www.vagrantup.com/) and then follow these steps:

1. Clone this repo into a `vagrant` folder at the root of your project.  So it's sitting alongside your `public`, `craft`, etc. folders:

		git clone https://github.com/enovatedesign/vagrant-craft.git vagrant

2. Grab the sub-modules:

		cd vagrant && git submodule update --init

3. Make a logs directory within the Vagrant folder (PHP errors will be put here):

		mkdir logs

4. Move the Vagrantfile to the root of your project:

		mv .Vagrantfile ../Vagrantfile

5. Install the vagrant-cachier plug-in (this makes provisioning boxes from scratch way faster):

		cd .. && vagrant plugin install vagrant-cachier

6. Get started (first run this takes 10 minutes after that about 3 minutes - thanks to `vagrant-cachier`:

		vagrant up

7. Restore your DB using phpMyAdmin by going to [http://localhost:9000/](http://localhost:9000/) (you'll be logged-in automatically).

8. Access your website at [http://localhost:8080/](http://localhost:8080/) and get to work!

You can use your hosts file to point [http://craft.dev:8080/](http://craft.dev:8080/) or any domain you fancy at the site.

Important gotchas
-----------------

1. Make sure your Craft database credentials are set correctly, e.g.:

		'.local' => array(
			'server'      => 'localhost',
			'user'        => 'root',
			'password'    => 'password',
			'database'    => 'DATABASE NAME HERE',
			'tablePrefix' => 'craft'
		)

2. The Nginx virtual host file assumes you are serving Craft from a folder called `public` within the root of your project, if that's not the case it can be edited in the file `/files/nginx/sites-available/default` at line 5.

3. For performance reasons the `craft/storage/runtime` folder is excluded from the Vagrant synced folder.  As Craft writes a lot of log entries here this has a major impact on the VM performance (particularly on Windows).  So excluding the folder makes the site run a LOT quicker but remember you need to `vagrant ssh` onto the VM to access the `craft.log` file.

4. In the Vagrantfile on lines 21 and 22 you may need to adjust the memory and CPU settings for the VM.

5. Using the `precise64.box` assumes you are running a 64-bit OS. I haven't tried but I assume you can switch to `precise32.box` if necessary (lines 10 and 11 of the Vagrantfile).

6. Running `vagrant destroy` is the virtual equivalent of throwing a computer out of the window so make sure you have a DB back-up first!

Nginx
-----

This is pre-configured to work with PHP-FPM and includes a Craft CMS friendly virtual host file and also another for phpMyAdmin.

The Craft CMS virtual host file includes some development friendly cache expiry settings too.

The server name that is presented to Craft CMS is "vagrant.local". This can be changed on line ~4 of the file `/files/nginx/sites-available/default` so it matches your multi-environment config.

MySQL
-----

The root password is "password".

phpMyAdmin
----------

This is accessible at [http://localhost:9000/](http://localhost:9000/) and will auto-login.

Log file location
-----------------

All Nginx and PHP-FPM log files are written to the `/logs/` folder.

As mentioned above the `craft/storage/runtime` folder is not synced so you need to `vagrant ssh` into the VM to view the `craft.log` stored there.

Craft CMS not included!
-----------------------

This doesn't actually include Craft CMS! Sorry, it's intended to be dropped *into* your Craft CMS project.

Troubleshooting
===============

You can un-comment line 20 in the Vagrantfile so that Virtualbox will launch a GUI so you can see the console for the VM as it launches and deal with any warnings/issues.

With thanks to...
=================

These repositories gave me a headstart on getting this done:

[https://github.com/experience/vagrant-craft-application](https://github.com/experience/vagrant-craft-application) by [Experience](https://github.com/experience)

[https://github.com/MikeRogers0/vagrant-nginx-wordpress-puppet](https://github.com/MikeRogers0/vagrant-nginx-wordpress-puppet) by [MikeRogers0](https://github.com/MikeRogers0)
