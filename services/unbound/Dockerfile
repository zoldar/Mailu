FROM alpine:3.8
# python3 shared with most images
RUN apk add --no-cache \
    python3 py3-pip git \
  && pip3 install --upgrade pip
# Shared layer between rspamd, postfix, dovecot, unbound and nginx
RUN pip3 install git+https://github.com/usrpro/MailuStart.git#egg=mailustart
# Image specific layers under this line
RUN apk add --no-cache unbound curl bind-tools \
  && curl -o /etc/unbound/root.hints https://www.internic.net/domain/named.cache \
  && chown root:unbound /etc/unbound \
  && chmod 775 /etc/unbound \
  && apk del --no-cache curl \
  && /usr/sbin/unbound-anchor -a /etc/unbound/trusted-key.key | true

COPY start.py /start.py
COPY unbound.conf /unbound.conf

EXPOSE 53/udp 53/tcp

CMD /start.py

HEALTHCHECK CMD dig @127.0.0.1 || exit 1
