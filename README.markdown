Puppet Dashboard
================

Puppet Dashboard is a web interface for [Puppet](http://www.puppetlabs.com/).
It can view and analyze Puppet reports, assign Puppet classes and parameters to
nodes, and view inventory data and backed-up file contents.

Dependencies
------------

* Ruby 2.2, 2.3, 2.4, 2.5
* MySQL >= 5.1 or PostgreSQL >= 9.0

Fast Install
------------

* Install prerequisites:
````
apt-get install git libmysqlclient-dev libpq-dev libsqlite3-dev ruby-dev libxml2-dev libxslt-dev nodejs
````
* Check out the code:
````
cd /usr/share && \
git clone https://github.com/sodabrew/puppet-dashboard.git && \
cd puppet-dashboard
````
* Create a MySQL database and user
````
mysql -p -e"CREATE DATABASE dashboard_production CHARACTER SET utf8;" && \
mysql -p -e"CREATE USER 'dashboard'@'localhost' IDENTIFIED BY 'my_password';" && \
mysql -p -e"GRANT ALL PRIVILEGES ON dashboard_production.* TO 'dashboard'@'localhost';"
````
* Set `max_allowed_packet = 32M` in your MySQL configuration
````
vim /etc/mysql/my.cnf
````
* Edit your `config/settings.yml` and `config/database.yml` files.
````
cp config/settings.yml.example config/settings.yml && \
cp config/database.yml.example config/database.yml && \
vim config/database.yml
````
* Install Puppet Dashboard Dependencies
````
gem install bundler && \
bundle install --deployment
````
* You need to create a secret for production and either set it via environment variable:
  `export SECRET_KEY_BASE=$(bundle exec rails secret)`
  or follow the instructions in config/secrets.yml to setup an encrypted secret. 
* Setup database and pre-compile assets
````
RAILS_ENV=production bundle exec rake db:setup && \
RAILS_ENV=production bundle exec rake assets:precompile
````
* Start Puppet Dashboard manually
````
RAILS_ENV=production bundle exec rails server
````
* Set up Puppet to be Dashboard-aware.
* Start the delayed job worker processes.
* You will find an initscript and other useful files for Debian in `ext/debian`

Production Environment
----------------------

Dashboard is currently configured to serve static assets when `RAILS_ENV=production`. In high-traffic
environments, you may wish to farm this out to Apache or nginx.  Additionally, you must explicitly
precompile assets for production using:

 * `SECRET_KEY_BASE=none RAILS_ENV=production bundle exec rails assets:precompile`

Contributing
------------

To contribute to this project, please read [CONTRIBUTING](CONTRIBUTING.md).  
A list of contributors is found in [CONTRIBUTORS](CONTRIBUTORS.md). Thanks!  
This project uses the [Silk icons](http://www.famfamfam.com/lab/icons/silk/) by Mark James.  Thank you!
