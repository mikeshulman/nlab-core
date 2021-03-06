nlab_core
---------

This repository contains the source code for the new version of the
nLab software.

nLab deployment
---------------

To deploy the nLab locally, do the following from the root directory of
nlab-core.

```bash
source deploy/nlab_local_environment_variables
./deploy/docker/docker_build.sh
./deploy/docker/docker_run.sh
```

This creates an nLab installation with an empty database with name set to the
value of the environment variable `NLAB_MYSQL_DATABASE_NAME`. To populate that
database from an SQL dump, wait a minute or two to give the MySQL database time
to come up, then set the environment variable `NLAB_DOCKER_SQL_FILE_PATH` in
`deploy/local_environment_variables` to the absolute file path of the SQL dump,
run

```bash
source deploy/nlab_local_environment_variables
```

again, and run the following in addition.

```bash
./deploy/populate_database.sh
```

If you receive an error such as

```
Lost connection to MySQL server at 'handshake: reading initial communication packet'
```

wait a little while and try again.

To deploy the nLab in production, follow all of these steps in the same way, but
use `nlab_production_environment_variables` instead.

nLab architecture
-----------------

1. All interaction with the nLab is via an nginx server.
2. The pages of the nLab are served statically by nginx, i.e. the nLab
itself is actually a static website.
3. The nLab is backed by a MySQL database which stores, for a given page,
metadata about this page, and page content in HTML (i.e. rendering from markdown
happens before database storage).
4. The static pages served by nginx are generated upon page edit, or manually,
by 'page_renderer.py' from the database.
5. The HTML page content stored in the database is rendered upon page edit from
markdown by 'source_renderer.py'.

nLab code testing
-----------------

Before pushing/merging to master, all tests must passed. The tests must be run
manually. See the README.md file in test/ for details on how to run the tests.
