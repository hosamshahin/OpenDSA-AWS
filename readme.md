Install OpenDSA-LTI manually 
- install dependences
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install libmysqlclient-dev

- intall ruby and rails
https://www.digitalocean.com/community/tutorials/how-to-install-ruby-on-rails-with-rvm-on-ubuntu-18-04
Ruby version 2.4
rails v 4 --no-document
gem install bundler:1.17.3 --no-document
- git clone OpenDSA
- git clone OpenDSA-LTI
- cd OpenDSA-LTI then bundle install 

- install nginx 
https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-18-04
- install passenger
https://www.phusionpassenger.com/library/install/nginx/install/oss/bionic/


- install mysql
https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-18-04
sudo apt-get -y install mysql-server-5.7

- create database
https://github.com/OpenDSA/OpenDSA-DevStack/blob/master/bootstrap.sh#L49


- install node
https://github.com/OpenDSA/OpenDSA-LTI#install-nodejs-and-bower

- configure rails app to connect to the database
- configure nginx and passenger to serve rails app
https://www.phusionpassenger.com/library/walkthroughs/deploy/ruby/ownserver/nginx/oss/bionic/deploy_app.html


- references
https://aws.amazon.com/blogs/devops/best-practices-for-deploying-applications-on-aws-cloudformation-stacks/

https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/deploying.applications.html


