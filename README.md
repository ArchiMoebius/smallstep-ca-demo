[smallstep](https://smallstep.com/)

## Quick Start

1. Clone this repository and change directory into it.

    Add ca.tft and index.tft to your local DNS resolver (/etc/hosts, pi-hole, etc.)

2. Run `docker-compose up`

3. Get the fingerprint from the CA by doing a `docker logs step-ca`.

    ``` 
    docker logs step-ca 

    No configuration file found at /home/step/config/ca.json

    Generating root certificate... 
    all done!

    Generating intermediate certificate... 
    all done!

    ✔ Root certificate: /home/step/certs/root_ca.crt
    ✔ Root private key: /home/step/secrets/root_ca_key
    ✔ Root fingerprint: 17096d30163602c2743f10032126f6a09a71cc0ea023879d980584cd4870c5f3  <---------------
    ✔ Intermediate certificate: /home/step/certs/intermediate_ca.crt
    ✔ Intermediate private key: /home/step/secrets/intermediate_ca_key
    ✔ Database folder: /home/step/db
    ✔ Default configuration: /home/step/config/defaults.json
    ✔ Certificate Authority configuration: /home/step/config/ca.json

    Your PKI is ready to go. To generate certificates for individual services see 'step help ca'.
    ``` 

    Add it to the `FINGERPRINT` environment variable in the `.env` file.

    ```

4. Run `docker-compose up -d`

5. Copy the root and intermediate certificates from the container to your host and add them to the hosts trust store. I installed Smallstep's CLI on my host and used it to install the certificates in my computers trust store. It packs them into a .pem file for me so I do not have to do this myself.

    `step-cli ca bootstrap --ca-url https://ca.tft:8443 --fingerprint <FINGERPRINT HERE> --install`