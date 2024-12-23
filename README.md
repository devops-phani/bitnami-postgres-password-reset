# bitnami-postgres-password-reset

Exec inside the postgres pod

```
kubectl exec -it <postgres pod name> -n <namespace> -- bash
```

Take the backup of /opt/bitnami/postgresql/conf/pg_hba.conf file content to notepad or to a temp file

```
cat /opt/bitnami/postgresql/conf/pg_hba.conf > /tmp/pg_hba.conf
```

Verify the backup content

```
cat /tmp/pg_hba.conf
```

```
host     all             all             0.0.0.0/0               md5
host     all             all             ::/0                    md5
local    all             all                                     md5
host     all             all        127.0.0.1/32                 md5
host     all             all        ::1/128                      md5
```


Update the /opt/bitnami/postgresql/conf/pg_hba.conf to accept the connection without password

```
echo "local    all             all                                     trust" >/opt/bitnami/postgresql/conf/pg_hba.conf
```

Run the following command to reload the pg_hba.conf file

```
/opt/bitnami/postgresql/bin/pg_ctl reload
```

Log into psql as postgres user and then update the user password to the password currently in the secret

```
psql -U postgres
```

```
ALTER USER postgres WITH PASSWORD 'new_password';
```
```
exit
```

Rewrite the /opt/bitnami/postgresql/conf/pg_hba.conf 

```
cat /tmp/pg_hba.conf > /opt/bitnami/postgresql/conf/pg_hba.conf
```

Run the following command to reload the pg_hba.conf file

```
/opt/bitnami/postgresql/bin/pg_ctl reload
```


Reference link
- https://jfrog.com/help/r/postgesql-for-helm-getting-sqlstate-28p01-password-authentication-failed-for-user-in-postgresql/how-to-reset-the-postgresql-password
