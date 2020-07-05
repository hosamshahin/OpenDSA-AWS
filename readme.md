Install OpenDSA-LTI manually 

- Install dependences

```
sudo apt-get update
sudo apt-get install -y curl gnupg build-essential libmysqlclient-dev

```

- Intall ruby and rails
https://www.digitalocean.com/community/tutorials/how-to-install-ruby-on-rails-with-rvm-on-ubuntu-18-04

```
gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
cd /tmp
curl -sSL https://get.rvm.io -o rvm.sh
cat /tmp/rvm.sh | bash -s stable --rails
source /home/ubuntu/.rvm/scripts/rvm
rvm install 2.4
rvm --default use 2.4

gem install rails -v 4 --no-document
gem install bundler:1.17.3 --no-document --no-rdoc --no-ri
\curl -sSL https://deb.nodesource.com/setup_10.x -o nodejs.sh
cat /tmp/nodejs.sh | sudo -E bash -
sudo apt-get update
sudo apt-get install -y nodejs
```

- Install nginx 
https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-18-04

```
sudo apt-get update
sudo apt-get install -y nginx
sudo ufw allow 'Nginx HTTP'
sudo ufw status
```

- Install passenger
https://www.phusionpassenger.com/library/install/nginx/install/oss/bionic/

```
sudo apt-get install -y dirmngr gnupg
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 561F9B9CAC40B2F7
sudo apt-get install -y apt-transport-https ca-certificates

sudo sh -c 'echo deb https://oss-binaries.phusionpassenger.com/apt/passenger bionic main > /etc/apt/sources.list.d/passenger.list'
sudo apt-get update
sudo apt-get install -y libnginx-mod-http-passenger

if [ ! -f /etc/nginx/modules-enabled/50-mod-http-passenger.conf ]; then sudo ln -s /usr/share/nginx/modules-available/mod-http-passenger.load /etc/nginx/modules-enabled/50-mod-http-passenger.conf ; fi
sudo ls /etc/nginx/conf.d/mod-http-passenger.conf

sudo service nginx restart
```

- Install mysql
https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-18-04

```
sudo apt-get update
sudo apt-get install -y mysql-server-5.7

sudo mysql <<SQL
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'root';
FLUSH PRIVILEGES;
SQL
```

- Create database
https://github.com/OpenDSA/OpenDSA-DevStack/blob/master/bootstrap.sh#L49

```
mysql -uroot -proot <<SQL
CREATE DATABASE opendsa DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
GRANT ALL PRIVILEGES ON opendsa.* to 'opendsa'@'localhost'  IDENTIFIED BY 'opendsa';
FLUSH PRIVILEGES;
SQL
```

- Git OpenDSA

```
cd ~
git clone --depth=1 https://github.com/OpenDSA/OpenDSA.git
cd OpenDSA
make pull
cd ~
git clone --depth=1 https://github.com/OpenDSA/OpenDSA-LTI.git
cd OpenDSA-LTI
bundle install --deployment --without development test
```

- Link OpenDSA to OpenDSA-LTI
```
ln -s /home/ubuntu/OpenDSA /home/ubuntu/OpenDSA-LTI/public

```

- Configure rails app to connect to the database
```
vi ~/OpenDSA-LTI/config/database.yml
# Add
production:
   adapter: mysql2
   database: opendsa
   username: opendsa
   password: opendsa
   host: localhost
   strict: false

development:
   adapter: mysql2
   database: opendsa
   username: opendsa
   password: opendsa
   host: localhost
   strict: false
   
# Then save

cd ~/OpenDSA-LTI
RAILS_ENV=production
bundle exec rake db:drop
bundle exec rake db:create
bundle exec rake db:schema:load
bundle exec rake db:populate

```

- Configure rails app secret file

```
cd ~/OpenDSA-LTI
bundle exec rake secret
vi ~/OpenDSA-LTI/config/secrets.yml

# Copy the following lines and replace secret_string with your new secret
production:
  secret_key_base: secret_string

staging:
  secret_key_base: secret_string

# Then save

chmod 700 config db
chmod 600 config/database.yml config/secrets.yml
```

- Deploy rails on AWS
https://www.phusionpassenger.com/library/walkthroughs/deploy/ruby/aws/nginx/oss/bionic/deploy_app.html

compile rails assets

```
bundle exec rake assets:precompile
```

- Configure nginx and passenger to serve rails app
https://www.phusionpassenger.com/library/walkthroughs/deploy/ruby/ownserver/nginx/oss/bionic/deploy_app.html

```
sudo vi /etc/nginx/nginx.conf

user ubuntu;
worker_processes auto;
pid /run/nginx.pid;
include /etc/nginx/modules-enabled/*.conf;

events {
        worker_connections 768;
        # multi_accept on;
}

http {
        ##
        # Basic Settings
        ##

        sendfile on;
        tcp_nopush on;
        tcp_nodelay on;
        keepalive_timeout 65;
        types_hash_max_size 2048;
        # server_tokens off;

        server_names_hash_bucket_size 64;
        # server_name_in_redirect off;

        include /etc/nginx/mime.types;
        default_type application/octet-stream;

        ##
        # SSL Settings
        ##

        ssl_protocols TLSv1 TLSv1.1 TLSv1.2; # Dropping SSLv3, ref: POODLE
        ssl_prefer_server_ciphers on;

        ##
        # Logging Settings
        ##

        access_log /var/log/nginx/access.log;
        error_log /var/log/nginx/error.log;

        ##
        # Gzip Settings
        ##

        gzip on;

        # gzip_vary on;
        # gzip_proxied any;
        # gzip_comp_level 6;
        # gzip_buffers 16 8k;
        # gzip_http_version 1.1;
        # gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;

        ##
        # Virtual Host Configs
        ##

        include /etc/nginx/conf.d/*.conf;
        include /etc/nginx/sites-enabled/*;
}

#mail {
#       # See sample authentication script at:
#       # http://wiki.nginx.org/ImapAuthenticateWithApachePhpScript
# 
#       # auth_http localhost/auth.php;
#       # pop3_capabilities "TOP" "USER";
#       # imap_capabilities "IMAP4rev1" "UIDPLUS";
# 
#       server {
#               listen     localhost:110;
#               protocol   pop3;
#               proxy      on;
#       }
# 
#       server {
#               listen     localhost:143;
#               protocol   imap;
#               proxy      on;
#       }
#}

# passenger-config about ruby-command 
# /home/ubuntu/.rvm/gems/ruby-2.4.9/wrappers/ruby

sudo vi /etc/nginx/conf.d/mod-http-passenger.conf

# Then put the following two lines 
passenger_root /usr/lib/ruby/vendor_ruby/phusion_passenger/locations.ini;
passenger_ruby /home/ubuntu/.rvm/gems/ruby-2.4.9/wrappers/ruby;

sudo service nginx restart
```

- Configure nginx
```
sudo vi /etc/nginx/sites-enabled/default
# copy the following 
server {
        listen 80;
        listen [::]:80;
        server_name ec2-3-215-77-210.compute-1.amazonaws.com;

        passenger_enabled on;
        passenger_ruby /home/ubuntu/.rvm/gems/ruby-2.4.9/wrappers/ruby;

        rails_env    production;
        root         /home/ubuntu/OpenDSA-LTI/public;

        # redirect server error pages to the static page /50x.html
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
}

sudo service nginx restart

```


- references
```
https://aws.amazon.com/blogs/devops/best-practices-for-deploying-applications-on-aws-cloudformation-stacks/

https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/deploying.applications.html
```

- Question 
```
1- create opendsa.org subdomain
2- generate and use cert in EC2
3- 
```