![Edubadges](logo.png)

# Timestamped Signed Open Badges - TSOB

## This Python3 service can be used to sign open badges.

Installation: (most simple instructions for CentOS 7)

    $ scl enable rh-python36 bash

    $ export SECRET_KEY="verylongsecretkey"

    $ cd <source_dir>

    $ pip install -r requirements.txt

    $ python manage.py runserver

The signing service will be started at http://127.0.0.1:8000/

## Installation using example Docker scripts

Example directory structure to build the edubadges Signing Service Docker containers:

    Working_dir: /var/docker/tsob/

    ├── config
    │   ├── nginx
    │   └── tsob
    ├── docker-compose.yml
    ├── docker-entrypoint.sh
    ├── Dockerfile
    ├── signing-service
    │   ├── manage.py
    │   ├── README.md
    │   ├── requirements.txt
    │   ├── research
    │   ├── templates
    │   └── tsob
    └── example_install.sh

* Create a similar setup using the directory structure above.
* Copy the docker files from the Github docker subdir to your working_dir.
* Copy the config files from config/nginx and config/tsob/
* Run the example_install.sh script.

Basic commands to build:

    $ cd /var/docker/tsob/
    $ git clone --single-branch -b master https://github.com/edubadges/signing-service
    $ cp /var/docker/tsob/config/tsob/settings_local.py /var/docker/tsob/signing-service/tsob/settings/settings_local.py
    $ cp /var/docker/tsob/config/tsob/development.py /var/docker/tsob/signing-service/tsob/settings/development.py
    $ cp /var/docker/tsob/config/tsob/production.py /var/docker/tsob/signing-service/tsob/settings/production.py
    $ docker-compose build
    $ docker-compose up -d

This will create the two docker containers.


# Why timestamp a Signed Open Badge?

An Open Badge offers the 'to whom' and 'what' of a claim. 

Digital signing represents the issuer.

But what still needs to be solved is the 'when'.

This 'when' is hard to guarantee in the future. 

A robust technology is needed to guarantee the issuance of an Open Badge

Timestamping can be a secure, long-term technique that guarantees the 'to whom', 'what' and 'from whom (issuer)' through time.

This repo contains research on a way timestamping can be implemented in Signed Open Badge
    
    * /timestamping

## Next to timestamping this repo also contains research specifically on:
 
### What Signing algorithm to use

Choice of signing algorith is important. A comparison is done based on the below characteristics:

1. Key size (in bytes) of (a) public & (b) private keys. 
2. Signature size (in bytes). 
3. Key generation speed (in ms). 
4. Signing speed (in ms). 
5. Verification speed (in ms). 
6. Possibility of storage on a Hardware Security Module (HSM). 
7. Security of the algorithm (in bits). 
8. Implementation difficulty. 
9. Exposure to side channel attacks. 
10. Collision resistance. 
11. Batch verification option. 
12. Quantum resistance (presumably)


    * /signature_algorithm

### What Signature standard to use

Signing standard that can be used are JWS Compact serialisation or JWS JSON serialisation.

In this part the two methods are compared specifically for Signing Open Badges

    * /jws_type


## Content
For each of the above research parts there is **presentation, diagram and source** material.


## TLDR:
 The research on Timestamped Signed Open Badges leads to the following advice.

**To issue future (quantum computing resistant) proof signed open badges:**

	* use 'JWS compact serialisation' as the method of signing (also defined in IMS global Open badge V2 standard).
	
	* use EdDSA (curve 25519) as the single signature algorithm for production/real use (RSA is fine for pilot usage too).
	
	* use anchoring of the Assertion hash to the Bitcoin blockchain (this needs a merkle-proof extension, which is currently already in use by some).

 
