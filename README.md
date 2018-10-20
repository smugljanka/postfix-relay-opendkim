Base image that contains the Postfix/OpenDKIM services (based on the Alpine Linux)
==============

run postfix/opendkim as docker services.

## Requirement
+ Docker 18.2 and higher

## Installation
1. Build image

	```bash
	$ sudo docker pull smugljanka/postfix-relay-dkim
	```

## Usage 
1. Set the following stack parameters 
   `POSTFIX_NETWORKS='127.0.0.0/8,...', POSTFIX_HOSTNAME=mx.<YOUR_FQDN>, POSTFIX_DOMAIN=<YOUR_FQDN> and <YOUR_DKIM_SELECTOR>`
   into the mta-stack.yml
2. Generate RSA keys for your domain
   ```bash
   $ sudo openssl rsa -in your.domain.com.priv -pubout >your.domain.com.pub
   ```
3. Setup your DNS - add DKIM record into your domain with the public key created above
4. Set path to your your.domain.com.priv key into the "domain-dkim-key" secret
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
+ The mentioned services are used as MTA for my web application
+ The "app-net" is external, it's created outside of the stack

## Reference
+ [Postfix Howto](http://www.postfix.org/)
+ [OpenDKIM Howto](http://opendkim.org/)
+ TBD

