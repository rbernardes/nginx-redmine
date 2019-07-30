# Ubuntu 18.04 + redmine + mariadb (with a self-signed SSL Certificate)
![redmine](https://github.com/rbernardes/nginx-redmine/blob/master/redmine.png?raw=true)

### Requirements:
https://github.com/rbernardes/nginx-modules

### Install required packages:
```
apt install build-essential mariadb-server ruby ruby-dev libmysqlclient-dev imagemagick libmagickwand-dev gpg2
```

### Install gpg2 keys:
```
gpg2 --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
```

### Run Ruby Version Manager installer:
```
curl -sSL https://get.rvm.io | bash -s stable
```

### Run scripts:
```
source /usr/local/rvm/scripts/rvm
echo '[[ -s "/usr/local/rvm/scripts/rvm" ]] && source "/usr/local/rvm/scripts/rvm"' >> ~/.bashrc
usermod -a -G rvm yourinstalluser
```

### Check RVM requirements:
```
rvm requirements
```

### Install/compile RVM:
```
rvm install 2.2.3
rvm use 2.2.3 --default
```

### Download Redmine (at this moment 4.0.4):
```
wget -O /var/www/redmine-4.0.4.tar.gz http://www.redmine.org/releases/redmine-4.0.4.tar.gz
cd /var/www
tar xvzf redmine-4.0.4.tar.gz
mv redmine-4.0.4 redmine
```

### Create and configure database:
```
mysql -u root -p
CREATE DATABASE redmine CHARACTER SET utf8mb4;
CREATE USER 'redmine'@'localhost' IDENTIFIED BY 'my_password';
GRANT ALL PRIVILEGES ON redmine.* TO 'redmine'@'localhost';
quit

cp redmine/config/database.yml.example redmine/config/database.yml
vi redmine/config/database.yml

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
cd /var/www/redmine
gem install bundler -v 1.17.3
bundle install --without development test
bundle exec rake generate_secret_token
RAILS_ENV=production bundle exec rake db:migrate
RAILS_ENV=production bundle exec rake redmine:load_default_data
```

### Create a self-signed SSL Certificate
```
openssl req -x509 -nodes -days 3650 -newkey rsa:2048 -keyout /etc/ssl/private/redmine.key -out /etc/ssl/certs/redmine.crt
openssl dhparam -out /etc/ssl/certs/dhparam.pem 2048
```

### Configure nginx domain (attached)
