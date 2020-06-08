FROM alpine:3.7

RUN apk update && apk upgrade && apk add build-base
RUN apk add ruby ruby-bundler ruby-dev
RUN rm -rf /var/cache/apk/*

ENV APP_HOME /app
RUN mkdir $APP_HOME
WORKDIR $APP_HOME

ADD Gemfile* $APP_HOME/
RUN bundle install
ADD . $APP_HOME

ENV COMMENT_DATABASE_HOST comment_db
ENV COMMENT_DATABASE comments

CMD ["puma"]
