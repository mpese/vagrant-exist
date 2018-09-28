# eXist on Vagrant

## Introduction

This is a Vagrant VM to run eXistdb (http://exist-db.org/) for developing 
the _Manuscript Pamphleteering in Early Stuart England_ (MPESE) project. The 
setup uses the eXist puppet module (https://github.com/eXist-db/puppet-existdb).

## Running

    vagrant up

It can take several minutes to setup the environment on the first run. When
finished, the eXist dashboard will be available:

http://localhost:9090/exist/

After the MPESE app has been deployed (see https://github.com/mpese/app), the
app will be proxied via NGINX and will be available on port 8000:

http://localhost:8000


## Set an admin password

The installation does not set an admin password. The app build script expects
it to be set, so set one via the User Manager:

http://localhost:9090/exist/apps/usermanager/index.html