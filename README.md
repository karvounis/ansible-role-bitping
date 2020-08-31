ansible-role-bitping
=========

Installs and configures Bitping on a Linux instance. https://www.bitping.com

It has being tested in:
- Ubuntu 20.04
- Ubuntu 18.04
- Ubuntu 16.04
- Debian 10
- Debian 9

Role Variables
--------------

Available variables are listed below, along with default values (see [defaults/main.yml](defaults/main.yml)):

#### Bitping Credentials

If any of the 5 next variables is empty, the role makes a call to the login API to fill these variables properly.
In order for the call to the Bitping login API to be successful, `bitping_credentials_email` and `bitping_credentials_password` must both be set!
No matter how many hosts you run this role against, the call to the Login API will only happen once.

If all of them are set, then the `credentials.json` ([credentials.json template](templates/credentials.json.j2)) file will be generated from the set values without making a request to the Bitping login API.

```yaml
bitping_credentials_email: "changeme@protonmail.com"
bitping_credentials_password: "cHAngem3!123"
bitping_credentials_id: ""
bitping_credentials_name: ""
bitping_credentials_token: ""
```

#### Linux specific

These variables define the Linux user and the group that is going to be used to execute the Bitping service.

```yaml
bitping_user: "bitping"
bitping_group: "bitping"
bitping_user_home: "/home/bitping"
```

The binary is going to be downloaded and unzipped at `{{ bitping_directory }}` and the `credentials.json` is going to be 
generated under the home folder of the linux user. 
The `bitping_systemd_file_path` variable defines the place where the systemd configuration ([template](templates/credentials.json.j2)) for the Bitping service is going to live.

```yaml
bitping_directory: "/opt/bitping"
bitping_credentials_file_path: "{{ bitping_user_home }}/.bitping/credentials.json"
bitping_systemd_file_path: "/etc/systemd/system/bitping.service"
```

#### URLs

These URLs are used to download Bitping and fetch the JWT token by logging the user to the Login API URL.
It is highly unlikely they are going to change in the near future. However, I decided against hardcoding them.
 
```yaml
bitping_download_url: "https://downloads.bitping.com/node/linux"
bitping_api_url: "https://api.bitping.com"
bitping_login_url: "{{ bitping_api_url }}/users/login"
```

Dependencies
------------

None.

Example Playbook
----------------

```yaml
- hosts: ubuntu
  vars:
    bitping_credentials_email: "your_bitping_email"
    bitping_credentials_password: "your_bitping_password"
  roles:
     - role: ansible-role-bitping
```

License
-------

MIT

Author Information
------------------

Role was created in 2020 by Evangelos Karvounis.
