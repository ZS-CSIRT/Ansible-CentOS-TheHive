TheHive
=========

This role will install an instance of the tool "The Hive" on the selected server.  
The role was designed to install "StandAlone" servers of "The Hive", it will not support installing "The Hive" on one server and the ELK DB on another server.  

If you are installing a pair of "The Hive" and "Cortex" servers, first install the "Cortex" server to get the API Key for the connection between "The Hive" and "Cortex".  
See Vault variables "cortex_user_key"  

Requirements
------------

This role was tested and validated for:

 - CentOS7

Role Variables
--------------

This role comes with 6 sets of variables:

 1) TheHive Configuration variables:  
    These variables are used to define how your "The Hive" instance will be configured.  
    The variables are:  
    HOSTNAME: choose a hostname for your server  
    FQDN: the Fully Qualified Domaine Name of your server  
    HIVE_IP: the IP address used by the "The Hive" server  
    HIVE_BASEURL: the base URL you will use to connect to the "The Hive" web interface  
    ORG_ID: The ID or name of your Organisation. Will be used in "The Hive" to configure the organisation profile.  
    HIVE_EMAIL: The email that the "The Hive" instance will impersonate when sending email notifications.  
    HIVE_CONF: The location of the "The Hive" configuration file (default: /etc/thehive/application.conf)  

 2) ElasticSearch configuration variables  
    These variables are used to define how the ElasticSearch DB engine used by "The Hive" will be configured.  
    The variables are:  
    ELK_REPO_FILE: The location of the yum repository file to be created to add the ELK repo to your yum configuration  
    ELK_CONF_FILE: The location of the ELK configuration file.  
    ELK_HOST: the IP address or URL where the ELK server is located. (By default this should be 127.0.0.1)  
    ELK_CLUSTER_NAME: The name you want to give to the ELK cluster  
    ELK_NODE_NAME: The name you want to give to the ELK node (You can have multiple nodes per clusters. This Ansible will not support multiple node setup)  
    ELK_INDEX: The name of your ELK index (Where all yout "The Hive" entries will be stored)  

 3) SSL Configruation variables  
    These variables define if you will make the "The Hive" instance run over SSL and with a Self-Signed or Imported certificate.  
    SSL: Whether to use SSL or not (true or false)  
    SSL_SELF_SIGNED: Whether to use a self signed certificate or import one from the ansible vault (true or false)  

 4) OpenSSL settings for cert  
    These variables are used to configure and generate the self signed certificates used by "The Hive"  
    The variables are:  
    OPENSSL_C: The country code  
    OPENSSL_ST: The area name  
    OPENSSL_L: The city name  
    OPENSSL_O: The organisation ID - This variable is by default populated by the ORG_ID variable  
    OPENSSL_CN: The common name - This variable is by default populated by the FQDN variable  
    HIVE_KEY: The certificate private key file name used by "The Hive"  
    HIVE_CSR: The certificate signing request file name used by "The Hive"  
    HIVE_CRT: The certificate file name used by "The Hive"  
    HIVE_CA:  

 5) LDAP Config  
    These variables are used to configure the LDAP connection for the "The Hive" user authentication  
    LDAP_PROVIDER: Set this variable to define which authentication provider you want to use. /!\ WARNING /!\ If you remove local, you cannot login with the local admin anymore!  
    LDAP_SERVERS: Defines the server to contact to perform the LDAP authentication challenge  
    LDAP_BIND_DN: The binding string to use to connect to your LDAP  
    LDAP_BASE_DN: The base DN to use to filter which users are allowed to connect to "The Hive" via the LDAP connection  
    LDAP_FILTER: The LDAP item to check the login user name against when performing the authentication challenge against LDAP.  

 6) Cortex configuration variables  
    These variables are used to configure the "Cortex" instance linked to the "The Hive".  
    CORTEX_INSTALL: True or False, this variable defines if you want to configure the "Cortex" settings in this run of Ansible.  
    CORTEX_IP: The IP address of the "Cortex" instance  
    CORTEX_FQDN: The "Cortex" FQDN to connect to.  

 7) Nginx configuration variable  
    This variable is used to define the file path of the Nginx conf file.  

 8) Ansible Vault Variables  
    These variables are stored in the ansible vault as they are sensitive and should not be stored in cleartext on your ansible server.  
    You can create a vault by using the command: ansible-vault create /path/to/vault/file  
    hive_http_secret: the secret string to use for the hive http sessions  
    ldap_bind_pw: the password for the bind user when using LDAP connection  
    cortex_user_key: the API key for the cortex user to link Cortex and TheHive.  

Dependencies
------------

Ansible 2.9+

Example Playbook
----------------

Example Playbook

    - name: HIVE
      hosts: hive001
      roles:
        - ansible-CentOS7-TheHive

    # ansible vault to use
      vars_files:
        - /etc/ansible/vaults/hive_vault

License
-------

EUPL v1.2

Author Information
------------------

Bertrand Varlet
