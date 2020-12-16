| gcc/g++           | clang/clang++     | visual studio     |
|-------------------|-------------------|-------------------|
| [![Build1][1]][10]| [![Build2][2]][10]| [![Build3][3]][10]|

[1]: https://travis-badge.herokuapp.com/repos/vmangos/core/branches/development/1?use_travis_com=true
[2]: https://travis-badge.herokuapp.com/repos/vmangos/core/branches/development/2?use_travis_com=true
[3]: https://travis-badge.herokuapp.com/repos/vmangos/core/branches/development/3?use_travis_com=true

[10]: https://travis-ci.com/github/vmangos/core


![C/C++ CI](https://github.com/vmangos/core/workflows/C/C++%20CI/badge.svg)

# Progressive Vanilla
This project is an independent continuation of the Elysium / LightsHope codebases, focused on delivering the most complete and accurate content progression system possible, including support for the patch appropriate game clients.

### Software to use
- OS: Ubuntu 18.04
- DB: 10.1.48-MariaDB-1~xenial
- ACE: libace 6.3.3
- TBB: libtbb 4.4
- Cmake: 3.5.1
- Compiler: gcc 5.4

### Currently supported builds
- 1.12.1.5875+
- 1.11.2.5464
- 1.10.2.5302
- 1.9.4.5086
- 1.8.4.4878
- 1.7.1.4695
- 1.6.1.4544

### Useful Links
- [Wiki](https://github.com/vmangos/wiki)
- [Script Editor](https://github.com/brotalnia/scripteditor)
- [Script Converter](https://github.com/vmangos/ScriptConverter)

## Running in docker

The supplied [docker-compose](docker-compose.yaml) and
[Dockerfile](docker/Dockerfile) files allow VMANGOS to be run from a collection
of docker containers.

### Setup

#### Configuration

Rename the
[data/cnf/realmd.conf.docker-example](data/cnf/realmd.conf.docker-example) file
to `data/cnf/realmd.conf` and the
[data/cnf/mangosd.conf.docker-example](data/cnf/mangosd.conf.docker-example) to
`data/cnf/mangosd.conf`. You can adjust the configuration values here, but they
will work by default.

#### Databases

Each database is run in a seperate MariaDB instance in a seperate docker
container:

* World database: `mangos_db`
* Realm database: `realmd_db`
* Logs database `logs_db`
* Character database: `character_db`

To initialize the databases youy must clone the databases project and execute
each SQL file in the appropriate container as shown below:

Note the path to the docker-compose file. If you're cloning this repository
inside the core repository you will not need the `-f` flag assuming you're in
the root of the repository.

Make sure to also change the database name, user name and password fi you're not
using the default values.

```sh
git clone https://github.com/the-hyjal-project/databases
cd databases
unzip world_*.zip
docker exec -i $(docker-compose -f ../../docker-compose.yml ps -q realmd_db) mysql -uroot --password=realmd realmd < auth.sql
docker exec -i $(docker-compose -f ../../docker-compose.yml ps -q characters_db) mysql -uroot --password=characers characters < chars.sql
docker exec -i $(docker-compose -f ../../docker-compose.yml ps -q logs_db) mysql -uroot --password=logs logs < logs.sql
docker exec -i $(docker-compose -f ../../docker-compose.yml ps -q mangos_db) mysql -uroot --password=mangos mangos < world.sql
```

Additional migrations may be required for each container. These can be found in
the[sql/migrations](sql/migrations) folder.

To import them run simmilar commands to the ones specified above keeping in mind
that \**\_001\_\**.sql goes in before \*\_002\_\*.sql. Each migration will be
prefixed with the appropriate database e.g. world\_000\_reserved\_name.sql goes
in the `mangos` database.

#### Map data

The server needs map files to run, which will be mounted from the
[data/maps](data/maps) folder. They can be imported as such (assuming cloning in
current directory).

```sh
git clone https://github.com/the-hyjal-project/data map_data
cp -r map_data/data/* data/maps
```
