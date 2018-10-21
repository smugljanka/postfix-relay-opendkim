Base image that contains the Postfix/OpenDKIM services (based on the Alpine Linux)
==============

run postfix/opendkim as docker services.

## Requirement
+ Docker 18.2 and higher

## Pull image from DockerHub

```bash
$ sudo docker pull smugljanka/postfix-relay-opendkim
```

## Usage 
1. Change the following stack parameters in mta-stack.yml 
   `
   POSTFIX_NETWORKS='127.0.0.0/8,...', POSTFIX_HOSTNAME=mx.<YOUR_FQDN>, 
   POSTFIX_DOMAIN=<YOUR_FQDN>, DKIM_SELECTOR=<YOUR_DKIM_SELECTOR>`
   
2. Generate RSA keys for your domain
```bash
 $ sudo openssl rsa -in your.domain.com.priv -pubout >your.domain.com.pub
```
3. Setup your DNS - create DKIM record in your domain - add the public key created above
4. Change "domain-dkim-key" secret - set path to your your.domain.com.priv key
```bash
   secrets:
     domain-dkim-key:
       file: /path/to/your/rsa-keys/your.domain.com.priv
```
4. Create docker stack
```bash
$ sudo docker stack deploy -c ./mta-stack.yml mta
```

## Note
+ The mentioned services are used as internal MTA for web application
+ The "app-net" network is external, it was created outside the stack

## Reference
+ [Postfix Howto](http://www.postfix.org/)
+ [OpenDKIM Howto](http://opendkim.org/)
+ TBD

