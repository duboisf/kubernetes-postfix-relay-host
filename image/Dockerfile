FROM alpine:3.8
MAINTAINER Chris Dornsife <chris@applariat.com>

RUN apk add --no-cache postfix rsyslog supervisor \
    && /usr/bin/newaliases \
    && postconf 'smtp_sasl_auth_enable = yes' \
    && postconf 'smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd' \
    && postconf 'smtp_sasl_security_options =' \
    # Override what you want here. The 10. network is for kubernetes
    && postconf 'mynetworks = 10.0.0.0/8,127.0.0.0/8,172.17.0.0/16' \
    # http://www.postfix.org/COMPATIBILITY_README.html#smtputf8_enable
    && postconf 'smtputf8_enable = no' \
    # This makes sure the message id is set. If this is set to no dkim=fail will happen.
    && postconf 'always_add_missing_headers = yes' \
    # We are going to run the container as readonly
    # The entrypoint is going to copy /etc/postfix-readonly to /etc/postfix
    # (/etc/postfix is going to be writable)
    && mv /etc/postfix /etc/postfix-readonly

COPY . /

EXPOSE 25

ENTRYPOINT [ "/tx-smtp-relay.sh" ]
