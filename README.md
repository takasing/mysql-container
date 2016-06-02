# mysql-container

If you do not want to install mysql with Homebrew, install in Vagrant.  
This repository provide you MySQL + Vagrant + Docker environment on local machine(Mac OS X).  

## Architecture
Docker(MySQL) + Docker(Volume container)  
↑  
Vagrant  
↑  
Mac OS X  

## Required
- VirtualBox
- Vagrant

## Initialize
```sh
vagrant up
```

### Restore database
at vagrant
```sh
docker exec -i mysql /bin/bash -c 'cat > ~/dump.sql' < share/dump.sql
docker exec -i mysql /bin/bash -c 'mysql -uroot < ~/dump.sql'
```

## Backup
coming soon
