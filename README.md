A knife-solo based Chef kitchen for a Django-ready host provisoning
===================================================================

*Tested on Ubuntu 12.04 Precise Pangolin x86 _only_.*

This kitchen was created to work along with `webdev-fab` set of Fabric tasks,
but you can use simple git hooks or other tools to deploy your project. It is
not a project deployment tool, but a host provisioning set of recipes and
sould be used up to the point your deployment tool comes into play.

It should be possible to use different node configs to set up additional
projects on the same host. The last provisioned node's settings may override
any previously set! For example, make sure you use the same PostgreSQL password
hash in every node configuration.

Configuration description
-------------

This is a collection of Chef recipes to set up single host production
environment to run a Django site. Django itself is run in WSGI mode by uWSGI
application container controlled by a Supervisor process manager. Nginx web
server connects to uWSGI UNIX socket in a project directory. PostgreSQL is
used with dedicated user/db for the project.

            Supervisor      --> Memcached
                |          |
                v          |
    Nginx --> uWSGI --> Django --> PostgreSQL


The following main tasks/apps are performed/installed:

 * en_US.UTF-8 locale
 * basic iptables statefull firewall rules
 * Nginx server with upload_progress_module
 * PostgreSQL database with additional tsearch2 data
 * Memcached
 * Supervisor with Django/uWSGI scripts
 * site-specific configs for Nginx and Supervisord

Testing in a VM
-------

It would be smart to test in a VM before running this complicated setup on a
real server.

 * To test this you may want to install Vagrant by hand (no gemset is provided
   for recent versions).
 * Install rvm && choose some ruby version.

We assume your SSH config knows what 'vagrantbox' hostname points to. You can
add something like this to make sure it would work:

    Host vagrantbox
      HostName 127.0.0.1
      Port 2222
      User vagrant
      IdentityFile ~/.ssh/vagrant_rsa

Prepare the host
----------------

Be sure to add a user allowed to run 'sudo'. It might be convinient to upload
your public SSH key too.

(Hint: you may use ~/.ssh/config entry like above to use different username,
port or key file.)

Prepare local kitchen environment
---------------------------------

    cd path/to/django-kitchen

Use RVM gemsets to isolate installed gems:

    rvm gemset create django-kitchen
    rvm gemset use django-kitchen

Install required ruby gems and chef tools:

    rm Gemfile.lock; bundle

Reinitialize knife-solo environment and generate knife.rb config:

    knife solo init .

Fetch required cookbooks:

    librarian-chef install

Use recipes
-----------

    vagrant up
    knife solo bootstrap vagrantbox

Subsequent usage
----------------

Each time before using knife or librarian-chef you should probably choose the
gemset it is installed in:

    rvm gemset use django-kitchen

If you do not use specific cookbook versions force Librarian-Chef to fetch
the latest versions from repositories:

    rm Cheffile.lock; librarian-chef install

Cook the box again after changing recipes

    knife solo cook vagrantbox

From this point you may use some project deployment tools like `webdev-fab`.

Resources
---------

 * [knife-solo](http://matschaffer.github.io/knife-solo/)
 * [django basic project template](https://github.com/mikek/django-basic-project-template)
 * [webdev-fab](https://github.com/mikek/webdev-fab)
 * [Opscode cookbooks](http://community.opscode.com/cookbooks)
 * [Librarian-Chef](https://github.com/applicationsonline/librarian-chef)
 * [Gemfile format](http://gembundler.com/v1.3/gemfile.html)
 * [RVM gemsests](https://rvm.io/gemsets/basics/)
