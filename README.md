# tidb-snowflake
Replicate from TiDB to Snowflake

## replicate snapshot data from TiDB to Snowflake

```bash
AWS_SDK_LOAD_CONFIG=true go run ./cmd/snapshot/main.go --storage s3://test/dump --table <database_name>.<table_name> --snowflake.account-id <organization>-<account> --snowflake.user <use_name> --snowflake.pass <password> --snowflake.database <database> --snowflake.schema <schema>
```

## replicate incremental data from TiDB to Snowflake

> **Warning**
> We do not support ddl replication yet. Any ddl operation may cause data loss.

```bash
# create a change feed
tiup cdc cli changefeed create --server=http://127.0.0.1:8300 --sink-uri="file:///tmp/test/cdc?protocol=csv&flush-interval=5m&file-size=268435456"

# start the replication
go run cmd/incremental/main.go --upstream-uri="file:///tmp/qiuyang-test/cdc?protocol=csv&flush-interval=5m&file-size=268435456" --downstream-uri="<use_name>:<password>@<organization>-<account>/<database>/<schema>?warehouse=<warehouse>"

# run any dml operation in tidb
...
```
