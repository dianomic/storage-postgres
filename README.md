# storage-postgres
This repository provides a tailored version of PostgreSQL used as a Storage Plugin for FogLAMP.

Download a tarball with the prepared version of PostgreSQL available here.
The tarball must be extracted in the git main folder. For example:

```
foglamp@foglamp-dev:~$ wget https://s3.amazonaws.com/foglamp/plugins/storage/postgres/pgsql-foglamp-9.6_201608131-x86_64.tgz
--2017-10-10 16:31:41--  https://s3.amazonaws.com/foglamp/plugins/storage/postgres/pgsql-foglamp-9.6_201608131-x86_64.tgz
Resolving s3.amazonaws.com (s3.amazonaws.com)... 52.216.230.125
Connecting to s3.amazonaws.com (s3.amazonaws.com)|52.216.230.125|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 13512593 (13M) [application/gzip]
Saving to: ‘pgsql-foglamp-9.6_201608131-x86_64.tgz’

pgsql-foglamp-9.6_201608131-x86_64.tgz     100%[=====================================================================================>]  12.89M   127KB/s    in 4m 18s

2017-10-10 16:35:59 (51.2 KB/s) - ‘pgsql-foglamp-9.6_201608131-x86_64.tgz’ saved [13512593/13512593]

foglamp@foglamp-dev:~$
```
