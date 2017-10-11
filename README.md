# storage-postgres
This repository provides a tailored version of PostgreSQL used as a Storage Plugin for FogLAMP.

Download a tarball with the prepared version of PostgreSQL available here:
* For Intel [x86_64](https://s3.amazonaws.com/foglamp/plugins/storage/postgres/pgsql-foglamp-9.6_201608131-x86_64.tgz).

The tarball must be extracted in the git main folder. 

This is the download and untar example for the x86 platform:
<pre>
foglamp@foglamp-dev:~$ <b>wget https://s3.amazonaws.com/foglamp/plugins/storage/postgres/pgsql-foglamp-9.6_201608131-x86_64.tgz</b>
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

There is a script in _packages/Debian/bin_ named _make_deb_. Once you have downloaded and extracted PostgreSQL for your favorite platform, execute the _make_deb_ script.

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

Install the PostgreSQL storage layer package as any other Debian package:
<pre>
foglamp@foglamp-test:~/Downloads$ <b>sudo dpkg -i foglamp-storage-postgres-00.01-9.6.201608131-x86_64.deb</b>
(Reading database ... 126439 files and directories currently installed.)
Preparing to unpack foglamp-storage-postgres-00.01-9.6.201608131-x86_64.deb ...
Unpacking foglamp-storage-postgres (00.01-9.6.201608131) over (00.01-9.6.201608131) ...
Setting up foglamp-storage-postgres (00.01-9.6.201608131) ...
foglamp@foglamp-test:~/Downloads$
</pre>

You can check if the packages is already installed and the name of the package with this command:
<pre>
foglamp@foglamp-test:~/Downloads$ <b>sudo dpkg -l | grep 'foglamp'</b>
ii  foglamp-storage-postgres           00.01-9.6.201608131                        amd64        PostgreSQL Storage Layer Plugin for FogLAMP
foglamp@foglamp-test:~/Downloads$
</pre>

If you want to uninstall the package, use the usual Debian command:
<pre>
foglamp@foglamp-test:~/Downloads$ <b>sudo dpkg -r foglamp-storage-postgres</b>
(Reading database ... 126439 files and directories currently installed.)
Removing foglamp-storage-postgres (00.01-9.6.201608131) ...
dpkg: warning: while removing foglamp-storage-postgres, directory '/usr/local' not empty so not removed
</pre>

## Starting, Stopping and Managing the Database Server

By default, the database server is installed in the _/usr/local/foglamp/plugins/storage/postgres/pgsql_.
The data directory, containing the system datafile and the socket file, is _/usr/local/foglamp/data/storage/postgres_

There is an script that can be used to administer the database server in _/usr/local/foglamp/plugins/storage/postgres/bin_. The name of the script is _foglamp.postgres_

The script executes these functions:
* **init**: Initialize the Database server (this action should not be necessary, since FogLAMP will execute it automatically)
* **start**: Start the Database server
* **status**: Check the status of the Database server
* **stop**: Stop the Database server

<pre>
foglamp@foglamp-test:/usr/local/foglamp/plugins/storage/postgres/bin$ <b>./foglamp.postgres init</b>
The files belonging to this database system will be owned by user "foglamp".
This user must also own the server process.

The database cluster will be initialized with locale "en_GB.UTF-8".
The default database encoding has accordingly been set to "UTF8".
The default text search configuration will be set to "english".

Data page checksums are disabled.

creating directory /usr/local/foglamp/data/storage/postgres/pgsql ... ok
creating subdirectories ... ok
selecting default max_connections ... 100
selecting default shared_buffers ... 128MB
selecting dynamic shared memory implementation ... posix
creating configuration files ... ok
running bootstrap script ... ok
performing post-bootstrap initialization ... ok
syncing data to disk ... ok

WARNING: enabling "trust" authentication for local connections
You can change this by editing pg_hba.conf or using the option -A, or
--auth-local and --auth-host, the next time you run initdb.

Success. You can now start the database server using:

    /usr/local/foglamp/plugins/storage/postgres/pgsql/bin/pg_ctl -D /usr/local/foglamp/data/storage/postgres/pgsql -l logfile start

foglamp@foglamp-test:/usr/local/foglamp/plugins/storage/postgres/bin$ <b>./foglamp.postgres status</b>
pg_ctl: no server running
foglamp@foglamp-test:/usr/local/foglamp/plugins/storage/postgres/bin$ <b>./foglamp.postgres start</b>
server starting
foglamp@foglamp-test:/usr/local/foglamp/plugins/storage/postgres/bin$ <b>./foglamp.postgres status</b>
pg_ctl: server is running (PID: 1422)
/usr/local/foglamp/plugins/storage/postgres/pgsql/bin/postgres "-D" "/usr/local/foglamp/data/storage/postgres/pgsql"
foglamp@foglamp-test:/usr/local/foglamp/plugins/storage/postgres/bin$ <b>./foglamp.postgres stop</b>
waiting for server to shut down.... done
server stopped
foglamp@foglamp-test:/usr/local/foglamp/plugins/storage/postgres/bin$ <b>./foglamp.postgres status</b>
pg_ctl: no server running
foglamp@foglamp-test:/usr/local/foglamp/plugins/storage/postgres/bin$
</pre>

