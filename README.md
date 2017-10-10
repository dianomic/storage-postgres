# storage-postgres
This repository provides a tailored version of PostgreSQL used as a Storage Plugin for FogLAMP.

Download a tarball with the prepared version of PostgreSQL available here:
* For Intel [x86_64](https://s3.amazonaws.com/foglamp/plugins/storage/postgres/pgsql-foglamp-9.6_201608131-x86_64.tgz).

The tarball must be extracted in the git main folder. 

This is the download and untar example for the x86 platform:
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

foglamp@foglamp-dev:~$foglamp@foglamp-dev:~$ cd storage-postgres/
foglamp@foglamp-dev:~/storage-postgres$ tar xzvf ~/pgsql-foglamp-9.6_201608131-x86_64.tgz
pgsql-x86_64/
pgsql-x86_64/share/
...
pgsql-x86_64/lib/libcom_err.so.3
foglamp@foglamp-dev:~/storage-postgres$
```

## Building the Debian Package

There is a script in _packages/Debian/bin_ named _make_deb_. Once you have downloaded and extracted PostgreSQL for your favorite platform, execute the _make_deb_ script.

```
foglamp@foglamp-dev:~/storage-postgres$ packages/Debian/bin/make_deb x86
The platform is set as x86_64
The package name is foglamp-storage-postgres-00.01-9.6.201608131-x86_64
Populating the package...Done.
Adding FogLAMP customization...Done.
Building the new package...
dpkg-deb: building package 'foglamp-storage-postgres' in 'foglamp-storage-postgres-00.01-9.6.201608131-x86_64.deb'.
Building Complete.
foglamp@foglamp-dev:~/storage-postgres$
```

The working directory and the newly built Debian package are in the _packages/Debian/build_ folder:
```
foglamp@foglamp-dev:~/storage-postgres$ ls -l packages/Debian/build/
total 8504
drwxrwxr-x 4 foglamp foglamp    4096 Oct 10 16:48 foglamp-storage-postgres-00.01-9.6.201608131-x86_64
-rw-r--r-- 1 foglamp foglamp 8701188 Oct 10 16:49 foglamp-storage-postgres-00.01-9.6.201608131-x86_64.deb
foglamp@foglamp-dev:~/storage-postgres$
```
