# Pcloud-dockerize

##Prerequisite

1. Create user calvinchu 
   1. https://www.digitalocean.com/community/tutorials/how-to-create-a-sudo-user-on-ubuntu-quickstart
2. Install Git
   1. [https://git-scm.com/book/zh-tw/v1/%E9%96%8B%E5%A7%8B-%E5%AE%89%E8%A3%9D-Git](https://git-scm.com/book/zh-tw/v1/開始-安裝-Git)
3. Install Docker CE
   1. https://docs.docker.com/install/linux/docker-ce/ubuntu/
4. Add calvinchu to docker group
   1. https://docs.docker.com/install/linux/linux-postinstall/
5. install docker-compose 
   1. https://docs.docker.com/compose/install/
6. Create SSH Key and paste public key to GitHub.
   1. https://help.github.com/en/articles/connecting-to-github-with-ssh
7. Install Cerbot(nginx, ubuntu 18.04 LTS)
   1. https://certbot.eff.org/lets-encrypt/ubuntubionic-nginx
   2. https://caloskao.org/ubuntu-use-certbot-to-automatically-update-lets-encrypt-certificate-authority/

## Deploy Sequence:

1. `mkdir pcloud` under linux `/home` folder.
2. enter pcloud folder `cd pcloud`
2. Add .env file `touch .env`  or modify .env
3. Copy environment variables and paste in .env file. 
4. `mkdir -p redis/data` to persist redis data.
5. `mkdir -p mongooseim/config` `mkdir -p mongooseim/log`  `mkdir -p mongooseim/mnesia` 

## SSO(Single Sign On) server

1. `git clone git@github.com:calvinchu8172/pcloud-sso-dockerize.git`
3. `mv pcloud-sso-dockerize pcloud-sso`
3. Add pcloud-sso/config/settings/production.yml
4. Add pcloud-sso/config/database.yml
   1. database.yml should includes mysql db and xmpp_db settings
5. Add pcloud-sso/config/mailer.yml
6. `mkdir -p pcloud-sso/tmp/pids`

##Portal and API Server

1. `git clone git@github.com:calvinchu8172/pcloud-portal-dockerize.git`
2. `mv pcloud-portal-dockerize pcloud-portal`
3. Add pcloud-portal/config/settings/production.yml
4. Add pcloud-portal/config/database.yml
   1. database.yml should includes mysql db and xmpp_db settings
5. Add pcloud-portal/config/mailer.yml
6. `mkdir -p pcloud-portal/tmp/pids`
7. Post device/3/register API to register device. Which is recorded in POSTMAN. 
8. Import `categories`, `certificates`, `devices`, `domains` and  `products`tables from local Database to `pcloud_production` database.

##Bots

1. `git clone git@github.com:calvinchu8172/pcloud-bots-dockerize.git`
2. `mv pcloud-bots-dockerize pcloud-bots`
3. Add pcloud-bots/config/bot_db_config.yml
4. Add pcloud-bots/config/bot_mail_config.yml
5. Add pcloud-bots/config/bot_quene_config.yml
6. Add pcloud-bots/config/bot_redis_config.yml
7. Add pcloud-bots/config/bot_route_config.yml
8. Add pcloud-bots/config/bot_xmpp_db_config.yml
9. Add pcloud-bots/config/god_config.yml
10. `mkdir -p pcloud-bots/log`
11. Import `user` and `last` table which includes device and bots accounts to` mongooseim_production` database.

##MongooseIM (XMPP Server)

1. Add mongoosein/config/ejabberd.cfg

##Dureading

1. `git clone git@github.com:calvinchu8172/dureading-dockerize.git`
2. `mv dureading-dockerize dureading`
3. Add dureading/config/settings/bot_db_config.yml
4. Add dureading/config/database.yml
5. `mkdir -p dureading/tmp/pids`
6. `docker-compose -f docker-compose-prod.yml run rake db:create db:migrate db:seed`


##Cerbot

1. If want to add another domain, excute `sudo cerbot --certonly`.
   1. Choose 1. nginx
   2. Enter another domain you want, like portal.lovefunthing.com.

##Sequence(Production)

1. `docker-compose -f docker-compose-prod.yml down`
2. `docker-compose -f docker-compose-prod.yml build`
3. `docker-compose -f docker-compose-prod.yml run sso rake db:create`
   1. SSO and Portal use the same DB migrate, so also can execute Portal migration `docker-compose -f docker-compose-prod.yml run portal rake db:create` and so are the following directives.
4. `docker-compose -f docker-compose-prod.yml run sso rake xmpp:db:create`
5. `docker-compose -f docker-compose-prod.yml run sso rake db:migrate`
6. `docker-compose -f docker-compose-prod.yml run sso rake xmpp:db:migrate`
7. `docker-compose -f docker-compose-prod.yml up `
8. Add bots and device accounts into DB and XMPP DB

## Warning

1. Every time Linode reboots, need to stop original Nginx in Linode. Otherwise the 443 port will be occupied and conflict with Docker Nginx
   1. `sudo service nginx stop`
   2. `sudo netstat -plntu` can see which port is occupied by which service.
2. Linode nano server (1G RAM) seems too small to deploy all above services.

