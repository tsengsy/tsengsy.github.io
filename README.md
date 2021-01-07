# tsengsy.github.io

Things to write about

## RDS IMPORT

- When importing database data into RDS/Aurora, follow this https://aws.amazon.com/premiumsupport/knowledge-center/error-1227-mysqldump/
- Use SED to remove DEFINER https://aws.amazon.com/premiumsupport/knowledge-center/definer-error-mysqldump/
- Then remove all references at the top such as

```
SET @MYSQLDUMP_TEMP_LOG_BIN = @@SESSION.SQL_LOG_BIN;
SET @@SESSION.SQL_LOG_BIN= 0;
SET @@GLOBAL.GTID_PURGED=/*!80000 '+'*/ '';
```

## Large number of domain SSL certs with only 1 ELB / ALB
