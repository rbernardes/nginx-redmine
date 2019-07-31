# Ubuntu 18.04 + redmine + mariadb (with a self-signed SSL Certificate)
![redmine](https://github.com/rbernardes/nginx-redmine/blob/master/redmine.png?raw=true)

### Requirements:
https://github.com/rbernardes/nginx-modules

### Install required packages:
```
# apt install build-essential mariadb-server ruby ruby-dev libmysqlclient-dev imagemagick libmagickwand-dev gnupg2
```

### Install gpg2 keys:
```
# gpg2 --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB

out:
gpg: WARNING: unsafe ownership on homedir '/home/rbernardes/.gnupg'
gpg: keybox '/home/rbernardes/.gnupg/pubring.kbx' created
gpg: key 105BD0E739499BDB: 6 signatures not checked due to missing keys
gpg: /home/rbernardes/.gnupg/trustdb.gpg: trustdb created
gpg: key 105BD0E739499BDB: public key "Piotr Kuczynski <piotr.kuczynski@gmail.com>" imported
gpg: key 3804BB82D39DC0E3: 106 signatures not checked due to missing keys
gpg: key 3804BB82D39DC0E3: public key "Michal Papis (RVM signing) <mpapis@gmail.com>" imported
gpg: no ultimately trusted keys found
gpg: Total number processed: 2
gpg:               imported: 2
```

### Run Ruby Version Manager installer:
```
# curl -sSL https://get.rvm.io | bash -s stable

out:
Downloading https://github.com/rvm/rvm/archive/1.29.9.tar.gz
Downloading https://github.com/rvm/rvm/releases/download/1.29.9/1.29.9.tar.gz.asc
gpg: WARNING: unsafe ownership on homedir '/home/rbernardes/.gnupg'
gpg: Signature made Wed 10 Jul 2019 05:31:02 AM -03
gpg:                using RSA key 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
gpg: Good signature from "Piotr Kuczynski <piotr.kuczynski@gmail.com>" [unknown]
gpg: WARNING: This key is not certified with a trusted signature!
gpg:          There is no indication that the signature belongs to the owner.
Primary key fingerprint: 7D2B AF1C F37B 13E2 069D  6956 105B D0E7 3949 9BDB
GPG verified '/usr/local/rvm/archives/rvm-1.29.9.tgz'
Creating group 'rvm'
Installing RVM to /usr/local/rvm/
Installation of RVM in /usr/local/rvm/ is almost complete:

  * First you need to add all users that will be using rvm to 'rvm' group,
    and logout - login again, anyone using rvm will be operating with `umask u=rwx,g=rwx,o=rx`.

  * To start using RVM you need to run `source /etc/profile.d/rvm.sh`
    in all your open shell windows, in rare cases you need to reopen all shell windows.
  * Please do NOT forget to add your users to the rvm group.
     The installer no longer auto-adds root or users to the rvm group. Admins must do this.
     Also, please note that group memberships are ONLY evaluated at login time.
     This means that users must log out then back in before group membership takes effect!
Thanks for installing RVM 🙏
Please consider donating to our open collective to help us maintain RVM.

👉  Donate: https://opencollective.com/rvm/donate
```

### Run scripts:
```
# source /usr/local/rvm/scripts/rvm
# echo '[[ -s "/usr/local/rvm/scripts/rvm" ]] && source "/usr/local/rvm/scripts/rvm"' >> ~/.bashrc
# usermod -a -G rvm yourinstalluser
```

### Check RVM requirements:
```
# rvm requirements

out:
Checking requirements for ubuntu.
Installing requirements for ubuntu.
Updating system...
Installing required packages: bison, libffi-dev, libgdbm-dev, libncurses5-dev, libsqlite3-dev, libyaml-dev, libreadline-dev........
Requirements installation successful.
```

### Install/compile RVM:
```
# rvm install 2.2.3

out:
Searching for binary rubies, this might take some time.
No binary rubies available for: ubuntu/18.04/x86_64/ruby-2.2.3.
Continuing with compilation. Please read 'rvm help mount' to get more information on binary rubies.
Checking requirements for ubuntu.
Removing undesired packages: libssl-dev...
Installing requirements for ubuntu.
Updating system...
Installing required packages: libssl1.0-dev...
Requirements installation successful.
Installing Ruby from source to: /usr/local/rvm/rubies/ruby-2.2.3, this may take a while depending on your cpu(s)...
ruby-2.2.3 - #downloading ruby-2.2.3, this may take a while depending on your connection...
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 12.7M  100 12.7M    0     0  7686k      0  0:00:01  0:00:01 --:--:-- 7682k
ruby-2.2.3 - #extracting ruby-2.2.3 to /usr/local/rvm/src/ruby-2.2.3.....
ruby-2.2.3 - #applying patch /usr/local/rvm/patches/ruby/2.2.3/fix_installing_bundled_gems.patch.
ruby-2.2.3 - #applying patch /usr/local/rvm/patches/ruby/2.2.3/openssl3.patch.
ruby-2.2.3 - #configuring.........................................................
ruby-2.2.3 - #post-configuration..
ruby-2.2.3 - #compiling....................................................................................................................................................................................................................|
ruby-2.2.3 - #installing.................
ruby-2.2.3 - #making binaries executable..
ruby-2.2.3 - #downloading rubygems-3.0.4
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  862k  100  862k    0     0  4070k      0 --:--:-- --:--:-- --:--:-- 4070k
ruby-2.2.3 - #extracting rubygems-3.0.4.....
ruby-2.2.3 - #removing old rubygems........
ruby-2.2.3 - #installing rubygems-3.0.4.........................................
ruby-2.2.3 - #gemset created /usr/local/rvm/gems/ruby-2.2.3@global
ruby-2.2.3 - #importing gemset /usr/local/rvm/gemsets/global.gems................................................................
ruby-2.2.3 - #generating global wrappers.......
ruby-2.2.3 - #gemset created /usr/local/rvm/gems/ruby-2.2.3
ruby-2.2.3 - #importing gemsetfile /usr/local/rvm/gemsets/default.gems evaluated to empty gem list
ruby-2.2.3 - #generating default wrappers.......
ruby-2.2.3 - #adjusting #shebangs for (gem irb erb ri rdoc testrb rake).
Install of ruby-2.2.3 - #complete
Please be aware that you just installed a ruby that requires 2 patches just to be compiled on an up to date linux system.
This may have known and unaccounted for security vulnerabilities.
Please consider upgrading to ruby-2.6.3 which will have all of the latest security patches.
Ruby was built without documentation, to build it run: rvm docs generate-ri
```

### Use RVM 2.2.3
```
# rvm use 2.2.3 --default

out:
Using /usr/local/rvm/gems/ruby-2.2.3
```

### Download Redmine (at this time 4.0.4):
```
# wget -O /var/www/redmine-4.0.4.tar.gz http://www.redmine.org/releases/redmine-4.0.4.tar.gz
# cd /var/www
# tar xvzf redmine-4.0.4.tar.gz
# mv redmine-4.0.4 redmine
```

### Create and configure database:
```
# mysql -u root -p
CREATE DATABASE redmine CHARACTER SET utf8mb4;
CREATE USER 'redmine'@'localhost' IDENTIFIED BY 'my_password';
GRANT ALL PRIVILEGES ON redmine.* TO 'redmine'@'localhost';
quit

# cp redmine/config/database.yml.example redmine/config/database.yml
# vi redmine/config/database.yml

out:
# Default setup is given for MySQL with ruby1.9.
# Examples for PostgreSQL, SQLite3 and SQL Server can be found at the end.
# Line indentation must be 2 spaces (no tabs).

production:
  adapter: mysql2
  database: redmine
  host: localhost
  username: redmine
  password: "my_password"
  encoding: utf8
  
[SHIFT+zz to save file]
```

### Create and configure database:
```
# cd /var/www/redmine
# gem install bundler -v 1.17.3

out:
Fetching bundler-1.17.3.gem
Successfully installed bundler-1.17.3
Parsing documentation for bundler-1.17.3
Installing ri documentation for bundler-1.17.3
Done installing documentation for bundler after 13 seconds
1 gem installed

# bundle install --without development test

:out
Don't run Bundler as root. Bundler can ask for sudo if it is needed, and installing your bundle as root will break this application for all non-root users on this machine.
The dependency tzinfo-data (>= 0) will be unused by any of the platforms Bundler is installing for. Bundler is installing for ruby but the dependency is only for x86-mingw32, x64-mingw32, x86-mswin32. To add those platforms to the bundle, run `bundle lock --add-platform x86-mingw32 x64-mingw32 x86-mswin32`.
Fetching gem metadata from https://rubygems.org/.............
Fetching gem metadata from https://rubygems.org/.
Resolving dependencies......
Fetching rake 12.3.3
Installing rake 12.3.3
Fetching concurrent-ruby 1.1.5
Installing concurrent-ruby 1.1.5
Fetching i18n 0.7.0
Installing i18n 0.7.0
Fetching minitest 5.11.3
Installing minitest 5.11.3
Fetching thread_safe 0.3.6
Installing thread_safe 0.3.6
Fetching tzinfo 1.2.5
Installing tzinfo 1.2.5
Fetching activesupport 5.2.3
Installing activesupport 5.2.3
Fetching builder 3.2.3
Installing builder 3.2.3
Fetching erubi 1.8.0
Installing erubi 1.8.0
Fetching mini_portile2 2.4.0
Installing mini_portile2 2.4.0
Fetching nokogiri 1.9.1
Installing nokogiri 1.9.1 with native extensions
...

# bundle exec rake generate_secret_token
# RAILS_ENV=production bundle exec rake db:migrate

out:
== 1 Setup: migrating =========================================================
-- create_table("attachments", {:options=>"ENGINE=InnoDB", :force=>true, :id=>:integer})
   -> 0.2716s
-- create_table("auth_sources", {:options=>"ENGINE=InnoDB", :force=>true, :id=>:integer})
   -> 0.6934s
-- create_table("custom_fields", {:options=>"ENGINE=InnoDB", :force=>true, :id=>:integer})
   -> 0.3254s
-- create_table("custom_fields_projects", {:options=>"ENGINE=InnoDB", :id=>false, :force=>true})
   -> 0.9031s
-- create_table("custom_fields_trackers", {:options=>"ENGINE=InnoDB", :id=>false, :force=>true})
   -> 0.2756s
-- create_table("custom_values", {:options=>"ENGINE=InnoDB", :force=>true, :id=>:integer})
   -> 0.2514s
-- create_table("documents", {:options=>"ENGINE=InnoDB", :force=>true, :id=>:integer})
   -> 0.2911s
-- add_index("documents", ["project_id"], {:name=>"documents_project_id"})
   -> 0.2345s
-- create_table("enumerations", {:options=>"ENGINE=InnoDB", :force=>true, :id=>:integer})
...

# RAILS_ENV=production bundle exec rake redmine:load_default_data

out:
Select language: ar, az, bg, bs, ca, cs, da, de, el, en, en-GB, es, es-PA, et, eu, fa, fi, fr, gl, he, hr, hu, id, it, ja, ko, lt, lv, mk, mn, nl, no, pl, pt, pt-BR, ro, ru, sk, sl, sq, sr, sr-YU, sv, th, tr, uk, vi, zh, zh-TW [en]
```

### Create a self-signed SSL Certificate
```
# openssl req -x509 -nodes -days 3650 -newkey rsa:2048 -keyout /etc/ssl/private/redmine.key -out /etc/ssl/certs/redmine.crt
# openssl dhparam -out /etc/ssl/certs/dhparam.pem 2048
```

### Configure nginx domain (attached)
