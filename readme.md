Install OpenDSA-LTI manually 

- Install dependences

```
sudo apt-get update
sudo apt-get install -y libmysqlclient-dev
```

- Intall ruby and rails
https://www.digitalocean.com/community/tutorial s/how-to-install-ruby-on-rails-with-rvm-on-ubuntu-18-04

```
gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
cd /tmp
curl -sSL https://get.rvm.io -o rvm.sh
cat /tmp/rvm.sh | bash -s stable --rails
source /home/ubuntu/.rvm/scripts/rvm
rvm install 2.4
rvm use 2.4
gem install rails -v 4 --no-document
gem install bundler:1.17.3 --no-document
cd /tmp
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
systemctl status nginx
sudo systemctl restart nginx
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
sudo /usr/bin/passenger-config validate-install
```

- Install mysql
https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-18-04

```
sudo apt-get update
sudo apt-get install -y mysql-server-5.7
```

- Create database
https://github.com/OpenDSA/OpenDSA-DevStack/blob/master/bootstrap.sh#L49

```
debconf-set-selections <<< 'mysql-server mysql-server/root_password password root'
debconf-set-selections <<< 'mysql-server mysql-server/root_password_again password root'
mysql -uroot -proot <<SQL
CREATE DATABASE opendsa DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
GRANT ALL PRIVILEGES ON opendsa.* to 'opendsa'@'localhost'  IDENTIFIED BY 'opendsa';
FLUSH PRIVILEGES;
SQL
```

- Git OpenDSA

```
git clone --depth=1 https://github.com/OpenDSA/OpenDSA.git
cd OpenDSA
make pull
cd ~
git clone --depth=1 https://github.com/OpenDSA/OpenDSA-LTI.git
cd OpenDSA-LTI
bundle install 
```

- configure rails app to connect to the database
- configure nginx and passenger to serve rails app
https://www.phusionpassenger.com/library/walkthroughs/deploy/ruby/ownserver/nginx/oss/bionic/deploy_app.html


- references
https://aws.amazon.com/blogs/devops/best-practices-for-deploying-applications-on-aws-cloudformation-stacks/

https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/deploying.applications.html


- Question 
1- create opendsa.org subdomain
2- generate and use cert in EC2
