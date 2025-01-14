# PostgreSQL system role
![CI Testing](https://github.com/linux-system-roles/postgresql/workflows/tox/badge.svg)

This role installs, configures, and starts PostgreSQL Server.

The role also optimizes the database server settings to improve performance.

The role currently works with PostgreSQL server 10 12 and 13.
## Role Variables
### postgresql_verison
Allow set up version of Postgresql server. This role supports Postgresql 10 12 and 13
```yaml
postgresql_version: "13"
```
### postgresql_password
Optionally, you can set up password for database super user `postgres` by default
there is not a password, datababase is accessible from `postgres` system account via UNIX socket.
users are encouraged to use ansible vault
```yaml
postgresql_password: !vault |
          $ANSIBLE_VAULT;1.2;AES256;dev
          ....
```
### postgresql_pg_hba_conf
A description of input variables that are not reqiured. Upstream configuration is used by default.
Usage of `postgresql_pg_hba_conf` causes replacement of default upstream configuration
```yaml
postgresql_pg_hba_conf:
  - type: local
    database: all
    user: all
    auth_method: peer
  - type: host
    database: all
    user: all
    address: '127.0.0.1/32'
    auth_method: ident
  - type: host
    database: all
    user: all
    address: '::1/128'
    auth_method: ident
```
### postgresql_server_conf
Usage of `postgresql_server_conf` adds defined values at the end of postgresql.conf.
So the default ones are overwritten.
```yaml
postgresql_server_conf:
  ssl: on
  shared_buffers: 128 MB
  huge_pages: try
```
### postgresql_ssl_enable
To set up ssl connection it's necessary to set up `postgresql_ssl_enable` variable and provide server certificate and key.
```yaml
postgresql_ssl_enable: true
```
### postgresql_cert_name
To specify certificate name use `postgresql_cert_name` variable.
You can copy your certificate to `/etc/pki/tls/certs/server.crt` and key to `/etc/pki/tls/private/server.key` or
you can also use certificate system role. For more detail see [`examples/`](examples).
```yaml
postgresql_cert_name: server
```
### postgresql_key_path
Optionaly you can specify path to server key using `postgresql_key_path` variable. The default value is
```yaml
postgresql_key_path: /etc/pki/tls/private
```
### postgresql_cert_path
Optionaly you can specify path to server cert using `postgresql_cert_path` variable. The default value is
```ymal
postgresql_cert_path: "/etc/pki/tls/certs"
```
### postgresql_input_file
For running SQL script define path to your SQL file using `postgresql_input_file`:
```yaml
postgresql_input_file: "/tmp/mypath/file.sql"
```
### postgresql_server_tuning
By default the system role makes server settings tuning based on system resources,
This functionality is enabled by default. For disabling it there is a possibility to
set up the `postgresql_server_tuning` variable.
```yaml
postgresql_server_tuning: false
```

More about usage could be found in [`examples/`](examples) directory


## Example Playbook


```yaml
- hosts: all
  vars:
    postgresql_version: "13"
    postgresql_password: !vault |
          $ANSIBLE_VAULT;1.2;AES256;dev
          ....

  roles:
    - linux-system-roles.postgresql
```

You can find more examples in the [`examples/`](examples) directory.

## License

MIT
