* Don't have any browser to manage your domains, servers and so on?
* Want to automate/script some tasks?
* Want to quickly make some administrative jobs on a domain/server?

## Disclaimer

* **Never ever** install this tool on servers (dedicated as VPS) you rent to OVH
* I decline all responsabilities of the usage made of this tool, there is no warranty
* I'm not affiliated in any way to the OVH company

## Prerequisites

### Build dependencies

* cmake
* (a C compiler)

### Run dependencies

* openssl
* libcurl
* libxml2
* libedit (autocompletion)
* libiconv
* gettext (optional)

## Install

* OVH Europe (compile with `-DAPI_BASE_URL="https://eu.api.ovh.com/1.0"`)
    + [Create your application](https://eu.api.ovh.com/createApp/)
    + [Create a token](https://eu.api.ovh.com/createToken/)
* OVH North America (compile with `-DAPI_BASE_URL="https://ca.api.ovh.com/1.0"`)
    + [Create your application](https://ca.api.ovh.com/createApp/)
    + [Create a token](https://ca.api.ovh.com/createToken/)
* So you Start Europe (compile with `-DAPI_BASE_URL="https://eu.api.soyoustart.com/1.0"`)
    + [Create your application](https://eu.api.soyoustart.com/createApp/)
    + [Create a token](https://eu.api.soyoustart.com/createToken/)
* So you Start North America (compile with `-DAPI_BASE_URL="https://ca.api.soyoustart.com/1.0"`)
    + [Create your application](https://ca.api.soyoustart.com/createApp/)
    + [Create a token](https://ca.api.soyoustart.com/createToken/)
* Kimsufi Europe (compile with `-DAPI_BASE_URL="https://eu.api.kimsufi.com/1.0"`)
    + [Create your application](https://eu.api.kimsufi.com/createApp/)
    + [Create a token](https://eu.api.kimsufi.com/createToken/)
* Kimsufi North America (compile with `-DAPI_BASE_URL="https://ca.api.kimsufi.com/1.0"`)
    + [Create your application](https://ca.api.kimsufi.com/createApp/)
    + [Create a token](https://ca.api.kimsufi.com/createToken/)
* Runabove (compile with `-DAPI_BASE_URL="https://api.runabove.com/1.0"`)
    + [Create your application](https://api.runabove.com/createApp/)
    + [Create a token](https://api.runabove.com/createToken/)

Fork/compile/install ovh-cli
```
git clone ...
cmake . \
    -DAPPLICATION_KEY="<your application key>" \
    -DAPPLICATION_SECRET="<your application secret>" \
    -DAPI_BASE_URL="https://eu.api.ovh.com/1.0"
make
(sudo) make install
```
(for now, these parameters have to be set at compile time)

Register your account(s):
```
# with your password instead of a valid consumer key
ovh account <nic-handle> password <password>
# with a valid and unexpirable consumer key instead of a password
ovh account <nic-handle> password "" <consumer key>
```

## Commands

* log on/off => enable/disable HTTP logging (file http.log in current directory)
* account
    * list => list all known OVH accounts
    * \<nic-handle>
        * add \<password or use empty string - "" - to not record it> (\<consumer key> expires in|at \<expiration date or delay>) => register a new account
        * delete => delete *account*
        * default => set default account
        * switch => change current *account*
* domain
    * check => list domains and when they expire
    * list => display all domains owned by the current account
    * \<domain name> ("zone" may be more appropriate)
        * export => export *domain*/zone in DNS format
        * refresh => refresh *domain* (generation of a new serial)
        * dnssec
            * status => print if DNSSEC is enabled/disabled for *domain*
            * enable => enable DNSSEC for *domain*
            * disable => disable DNSSEC for *domain*
        * record ("subdomain" may be more appropriate) (TODO: find a more convenient way to manage empty subdomain name - SPF, NS, ... ?)
            * list => display all subdomains of *domain*
                * nocache => fetch data from OVH instead of using internel cache (avoid restarting ovh-cli shell)
                * type \<one of: A, AAAA, CNAME, LOC, MX, NAPTR, NS, PTR, SPF, SRV, SSHFP, TXT> => list only records of the given *type*
            * \<record name>
                * add \<target> type \<one of: A, AAAA, CNAME, LOC, MX, NAPTR, NS, PTR, SPF, SRV, SSHFP, TXT> => add *subdomain* to *domain*
                * delete => delete *subdomain* of *domain* (TODO: non unique subdomain names are all deleted)
                * update => update *subdomain* of *domain* (TODO: can only update subdomains which are not ambiguous)
                    * ttl \<ttl> => change TTL value (in minutes)
                    * name \<name> => rename *subdomain*
                    * value \<value> => alter value/target of *subdomain*
* dedicated
    * check => list servers and when they expire
    * list => list all servers associated to the current account
    * \<server name>
        * reboot => **hard** reboot of *server*
        * boot
            * list => list all available boots for *server*
            * show => display current boot for *server*
            * \<boot name> => set boot for next (re)boots of *server*
        * reverse
            * delete => remove reverse name of *server*
            * set \<reverse> => define reverse name of *server*
* hosting
    * list => list all hosted website
    * \<hosting>
        * user list => list (FTP & SSH) users of *hosting*
        * \<user>
            * delete => remove *user* from *hosting*
        * cron list => list cron jobs for *hosting*
        * domain list => list domains associated to *hosting*
        * \<domain>
            * delete => unlink *domain* to *hosting*
            * add \<path> => link *domain* to *hosting* through *path*
        * database list => list databases associated to *hosting*
        * \<database>
            * delete => drop *database* associated to *hosting*
* credentials
    * list => list all credentials used to connect to any OVH application with current account
    * flush => remove all credentials used with current account
* key
    * list => list global SSH keys
    * \<name>
        * delete => remove SSH key named *name*
        * add \<ssh key> => register *ssh key* as *name*
        * default <on/off> => (un)define SSH key named *name* as default (default = the one used in rescue mode)
* me => display personal informations associated to the current account (name, address, etc)
* help
* quit
