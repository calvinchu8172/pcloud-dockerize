# pcloud-dockerize

## Reminds:

1. `mkdir pcloud` under linux `/home` folder.
2. enter pcloud folder `cd pcloud`

2. Add .env file `touch .env` . 
3. Copy environment variables and paste in .env file. 
4. `mkdir -p redis/data` to persist redis data.
5. `mkdir -p mongooseim/log`  `mkdir -p mongooseim/mnesia` 

### SSO(Single Sign On) server

1. `git clone git@github.com:calvinchu8172/pcloud-sso-dockerize.git`
3. `mv pcloud-sso-dockerize pcloud-sso`
3. Add pcloud-sso/config/settings/production.yml
4. Add pcloud-sso/config/database.yml
   1. database.yml should includes mysql db and xmpp_db settings
5. Add pcloud-sso/config/mailer.yml

### Portal and API Server

1. `git clone git@github.com:calvinchu8172/pcloud-portal-dockerize.git`
2. `mv pcloud-portal-dockerize pcloud-portal`
3. Add pcloud-portal/config/settings/production.yml
4. Add pcloud-portal/config/database.yml
   1. database.yml should includes mysql db and xmpp_db settings
5. Add pcloud-portal/config/mailer.yml

###Bots

1. `git clone git@github.com:calvinchu8172/pcloud-portal-dockerize.git`
2. `mv pcloud-bots-dockerize pcloud-bots`
3. Add pcloud-bots/config/bot_db_config.yml
4. Add pcloud-bots/config/bot_mail_config.yml
5. Add pcloud-bots/config/bot_quene_config.yml
6. Add pcloud-bots/config/bot_redis_config.yml
7. Add pcloud-bots/config/bot_route_config.yml
8. Add pcloud-bots/config/bot_xmpp_db_config.yml
9. Add pcloud-bots/config/god_config.yml

### MongooseIM (XMPP Server)

1. Add mongoosein/config/ejabberd.cfg

