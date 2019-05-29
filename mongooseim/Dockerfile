FROM mongooseim/mongooseim-docker:1.5.1

ENV LANG C.UTF-8
# 指定時區，否則會用 GMT
ENV TZ Asia/Taipei
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# RUN apt-get update -qq && apt-get install -y build-essential imagemagick libmagickwand-dev qt5-default libqt5webkit5-dev nodejs redis-server xvfb mysql-client

# ENV APP_HOME /home/app/rails-app

ENV DOCKERIZE_VERSION v0.6.1

RUN wget https://github.com/jwilder/dockerize/releases/download/$DOCKERIZE_VERSION/dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
    && tar -C /usr/local/bin -xzvf dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
    && rm dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz

# ENV APP_HOME /home/app/rails-app

# WORKDIR $APP_HOME

COPY ./mongooseim/config/ejabberd.cfg  /opt/mongooseim/rel/mongooseim/etc/ejabberd.cfg
# RUN mkdir -p $APP_HOME
# COPY ./pcloud-sso $APP_HOME
# WORKDIR $APP_HOME

# expose xmpp, rest, s2s, epmd, distributed erlang
EXPOSE 5222 5280 5269 4369 9100