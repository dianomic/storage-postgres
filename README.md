# storage-postgres
This repository provides the tools to create a Debian package containing a tailored version of PostgreSQL used as a Storage Plugin for FogLAMP.

The objective is to provide a ready-built storage service plugin for FogLAMP for development or for installations in environments where the standard FogLAMP package cannot be used. In this README we will explain how to create, install and use the Debian package.

First, you need to download a tarball with the pre-built version of PostgreSQL, available here:

* For Intel [x86_64](https://s3.amazonaws.com/foglamp/plugins/storage/postgres/pgsql-foglamp-9.6_201608131-x86_64.tgz).
* For ARM   [amrhf](https://s3.amazonaws.com/foglamp/plugins/storage/postgres/pgsql-foglamp-9.6_201608131-armhf.tgz).

The tarball must be extracted in the git main folder. 

This is the download and untar example for the x86 platform:
<pre>
foglamp@foglamp-dev:~$ <b>wget https://s3.amazonaws.com/foglamp/plugins/storage/postgres/pgsql-foglamp-9.6\_201608131-x86_64.tgz</b>
--2017-10-10 16:31:41--  https://s3.amazonaws.com/foglamp/plugins/storage/postgres/pgsql-foglamp-9.6_201608131-x86_64.tgz
Resolving s3.amazonaws.com (s3.amazonaws.com)... 52.216.230.125
Connecting to s3.amazonaws.com (s3.amazonaws.com)|52.216.230.125|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 13512593 (13M) [application/gzip]
Saving to: ‘pgsql-foglamp-9.6_201608131-x86_64.tgz’

pgsql-foglamp-9.6_201608131-x86_64.tgz     100%[=====================================================================================>]  12.89M   127KB/s    in 4m 18s

2017-10-10 16:35:59 (51.2 KB/s) - ‘pgsql-foglamp-9.6_201608131-x86_64.tgz’ saved [13512593/13512593]

foglamp@foglamp-dev:~$ <b>cd storage-postgres/</b>
foglamp@foglamp-dev:~/storage-postgres$ <b>tar xzvf ~/pgsql-foglamp-9.6_201608131-x86_64.tgz</b>
pgsql-x86_64/
pgsql-x86_64/share/
...
pgsql-x86_64/lib/libcom_err.so.3
foglamp@foglamp-dev:~/storage-postgres$
</pre>


## Building the Debian Package

In order to build the pacakge, use a script in _packages/Debian/bin_ named _make\_deb_. Once you have downloaded and extracted PostgreSQL for your favorite platform, execute the _make\_deb_ script.

<pre>
foglamp@foglamp-dev:~/storage-postgres$ <b>packages/Debian/bin/make_deb x86</b>
The platform is set as x86_64
The package name is foglamp-storage-postgres-00.01-9.6.201608131-x86_64
Populating the package...Done.
Adding FogLAMP customization...Done.
Building the new package...
dpkg-deb: building package 'foglamp-storage-postgres' in 'foglamp-storage-postgres-00.01-9.6.201608131-x86_64.deb'.
Building Complete.
foglamp@foglamp-dev:~/storage-postgres$
</pre>

The working directory and the newly built Debian package are in the _packages/Debian/build_ folder:
<pre>
foglamp@foglamp-dev:~/storage-postgres$ <b>ls -l packages/Debian/build</b>
total 8504
drwxrwxr-x 4 foglamp foglamp    4096 Oct 10 16:48 foglamp-storage-postgres-00.01-9.6.201608131-x86_64
-rw-r--r-- 1 foglamp foglamp 8701188 Oct 10 16:49 foglamp-storage-postgres-00.01-9.6.201608131-x86_64.deb
foglamp@foglamp-dev:~/storage-postgres$
</pre>


## Installing and Uninstalling the Package

As prerequisite to the installation of the package, you should have FogLAMP installed, normally using the _make install_ command from your newly built FogLAMP.

Assuming you have created the PostgreSQL storage layer package, you can install it as any other Debian package:
<pre>
foglamp@foglamp-test:~/storage-postgres/packages/Debian/build$ <b>sudo dpkg -i foglamp-storage-postgres-00.01-9.6.201608131-x86_64.deb</b>
(Reading database ... 126439 files and directories currently installed.)
Preparing to unpack foglamp-storage-postgres-00.01-9.6.201608131-x86_64.deb ...
Unpacking foglamp-storage-postgres (00.01-9.6.201608131) over (00.01-9.6.201608131) ...
Setting up foglamp-storage-postgres (00.01-9.6.201608131) ...
foglamp@foglamp-test:~/storage-postgres/packages/Debian/build$
</pre>

You can check if the package is already installed with this command:
<pre>
foglamp@foglamp-test:~/storage-postgres/packages/Debian/build$ <b>cd</b>
foglamp@foglamp-test:~$ <b>sudo dpkg -l | grep 'foglamp'</b>
ii  foglamp-storage-postgres           00.01-9.6.201608131                        amd64        PostgreSQL Storage Layer Plugin for FogLAMP
foglamp@foglamp-test:~$
</pre>

To uninstall the package, use the usual Debian command:
<pre>
foglamp@foglamp-test:~$ <b>sudo dpkg -r foglamp-storage-postgres</b>
(Reading database ... 126439 files and directories currently installed.)
Removing foglamp-storage-postgres (00.01-9.6.201608131) ...
dpkg: warning: while removing foglamp-storage-postgres, directory '/usr/local' not empty so not removed
foglamp@foglamp-test:~$
</pre>

## Starting, Stopping and Managing the Database Server

Note that the management of the database server is automatically performed by FogLAMP. Here we are presenting the scripts used by FogLAMP in order to facilitate the debugging that developers may need to perform.

By default, the database server is installed in _/usr/local/foglamp/plugins/storage/postgres/pgsql_.
The data directory, containing the system datafile and the socket file, is _/usr/local/foglamp/data/storage/postgres_.

The script _/usr/local/foglamp/plugins/storage/postgres/bin/foglamp.postgres_ is used to administer the database server.

The script executes these functions:
* **reset**: Reset the Database server. With this command, all the data in the database server will be removed.
* **start**: Start the Database server
* **status**: Check the status of the Database server
* **stop**: Stop the Database server
* **restart**: Stop and then start the Database server
* **help**: Show the help page

The _start_ argument passed to the script, automatically initializes the database server:
<pre>
foglamp@foglamp-test:/usr/local/foglamp/plugins/storage/postgres/bin$ <b>./foglamp.postgres start</b>
Cleaning the Root Data directory...
Initializing the Data directory...

WARNING: enabling "trust" authentication for local connections
You can change this by editing pg_hba.conf or using the option -A, or
--auth-local and --auth-host, the next time you run initdb.
waiting for server to start.... done
server started
Building the metadata for the FogLAMP Plugin...
Build completed.
foglamp@foglamp-test:/usr/local/foglamp/plugins/storage/postgres/bin$ <b>./foglamp.postgres status</b>
FogLAMP PostgreSQL plugin running.
foglamp@foglamp-test:/usr/local/foglamp/plugins/storage/postgres/bin$ <b>./foglamp.postgres stop</b>
waiting for server to shut down.... done
server stopped
foglamp@foglamp-test:/usr/local/foglamp/plugins/storage/postgres/bin$ <b>./foglamp.postgres status</b>
FogLAMP PostgreSQL plugin not running.
foglamp@foglamp-test:/usr/local/foglamp/plugins/storage/postgres/bin$ <b>./foglamp.postgres start</b>
waiting for server to start.... done
server started
foglamp@foglamp-test:/usr/local/foglamp/plugins/storage/postgres/bin$ <b>./foglamp.postgres reset</b>
This script will remove all data stored in the server.
Enter YES if you want to continue: <b>YES</b>
waiting for server to shut down.... done
server stopped
Cleaning the Root Data directory...
Initializing the Data directory...

WARNING: enabling "trust" authentication for local connections
You can change this by editing pg_hba.conf or using the option -A, or
--auth-local and --auth-host, the next time you run initdb.
FogLAMP PostgreSQL plugin reset.
foglamp@foglamp-test:/usr/local/foglamp/plugins/storage/postgres/bin$ <b>./foglamp.postgres start</b>
waiting for server to start.... done
server started
Building the metadata for the FogLAMP Plugin...
Build completed.
foglamp@foglamp-test:/usr/local/foglamp/plugins/storage/postgres/bin$ <b>./foglamp.postgres status</b>
FogLAMP PostgreSQL plugin running.
foglamp@foglamp-test:/usr/local/foglamp/plugins/storage/postgres/bin$
</pre>

The database is also accessible with the _psql_ command line utility, available in _/usr/local/foglamp/plugins/storage/postgres/pgsql/bin_. 

Please consider the defaults:
* The socket file is in /usr/local/foglamp/data/storage/postgres
* The user _foglamp_ (no password)
* The database is _postgres_

<pre>
foglamp@foglamp-test:/usr/local/foglamp/plugins/storage/postgres/pgsql/bin$ <b>/usr/local/foglamp/plugins/storage/postgres/bin/foglamp.postgres start</b>
waiting for server to start.... done
server started
foglamp@foglamp-test:/usr/local/foglamp/plugins/storage/postgres/pgsql/bin$ <b>./psql -U foglamp postgres -h /usr/local/foglamp/data/storage/postgres</b>
psql.bin (9.6.3)
Type "help" for help.

Cannot read termcap database;
using dumb terminal settings.
postgres=# <b>^D\q</b>
foglamp@foglamp-test:/usr/local/foglamp/plugins/storage/postgres/pgsql/bin$
</pre>

## Known Issues

### Executing psql on ARM
In order to execute psql on ARM, an extra set of libraries must be added to the path:

<pre>
foglamp@foglamp-dev:~$ <b>LD_LIBRARY_PATH=/usr/local/foglamp/plugins/storage/postgres/pgsql/lib:/usr/local/foglamp/plugins/storage/postgres/pgsql/lib/arm-linux-gnueabihf</b>
foglamp@foglamp-dev:~$ <b>export LD_LIBRARY_PATH</b>
foglamp@foglamp-dev:~$ <b>/usr/local/foglamp/plugins/storage/postgres/pgsql/bin/psql -h /usr/local/foglamp/data/storage/postgres -U foglamp postgres</b>
psql (9.6.3)
Type "help" for help.

postgres=# <b>\q</b>
foglamp@foglamp-dev:~$
</pre>

This issue will be fixed in the future.

