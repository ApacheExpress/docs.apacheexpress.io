# Database Access

A feature of Apache 2 known to few is
[mod_dbd](https://httpd.apache.org/docs/2.4/mod/mod_dbd.html).
Using that you can configure a SQL database connection within the 
[Apache.conf](https://github.com/AlwaysRightInstitute/mod_swift/blob/master/apache.conf#L79)
and use that within all your Apache modules/handlers.
Apache does proper and reliable pooling and connection management.

2019-08-14: Homebrew has removed support for apr-util DBD drivers except
SQLite:
[PR #31799](https://github.com/Homebrew/homebrew-core/pull/31799/commits/584a9faa5c2decf32f25bb9d5f028395bb93ab5f).
To fix that, you can manually install `apr-util` with the `--with-pgsql`
option (or any other driver you want to build).
Then just hack-link the driver you want into the Homebrew install, e.g.:
```
pushd /usr/local//opt/apr-util/libexec/lib/apr-util-1/
ln -s /usr/local/apr/lib/apr-util-1/apr_dbd_pgsql*.so .
```

ApacheExpress comes with the `mod_dbd` middleware which hooks up the Swift
[ZeeQL](http://zeeql.io/) database access toolkit with Apache.
ZeeQL provides convenient low level database access as well as high level
object relation mapping feature.
Database schemas can be provided directly
from within Swift code,
using CoreData models,
and you can even automatically generate them from existing databases using
ZeeQL's extensive schema reflection feature.

> WORK IN PROGRESS, STAY TUNED

## mod_dbd Configuration

To access a database from within Apache the database connection parameters
need to be configured in the Apache configuration.
Depending on how the ApacheExpress application was prepared, 
that may be already setup.

If not, the easiest way to do this is to use a mod_swift
[configuration template](http://docs.mod-swift.org/configtemplates/).
If there is none, simply create a file called `$module-template.conf`,
e.g. `mods_hellodb-template`.conf.
Example:
```
LoadModule dbd_module %APACHE_MODULE_DIR%/mod_dbd.so

<IfModule dbd_module>
  DBDriver  sqlite3
  DBDParams "%SRCROOT%/data/MyDatabase.sqlite3"
</IfModule>
```

PostgreSQL example:
```
<IfModule dbd_module>
  DBDriver  pgsql
  DBDParams "host=127.0.0.1 port=5432 dbname=OGo user=OGo password=OGo"

  # Connection Pool Management
  DBDMin      1
  DBDKeep     2
  DBDMax     10
  DBDExptime 60
</IfModule>
```

If you want to run PostgreSQL on Homebrew, make sure you have it installed,
e.g.:

    brew install apr-util --with-postgresql --with-sqlite

> WORK IN PROGRESS, STAY TUNED
