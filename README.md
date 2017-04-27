# Docker Omnibus

Dockerfile for building Netcool/OMNIbus docker containers.

## Building 

Dockerfiles are using hostname netcool.install (see Dockerfile) to download installation images. To successfully build docker images, you need to change the INSTALLHOST environment value located in Dockerfile to point to your location of IBM Netcool/OMNIbus installation files. Another option is to add the _netcool.install_ hostname to your local DNS server (if possible) and start a simple http server with your installation images such as python's SimpleHTTPServer.

Optionally you can remove installation manager stuff (directories and install data) from image by uncommenting appropriate lines.

## Running

You can start the stack using docker-compose and provided docker-compose.yml file. This starts up single Object Server named AGG and a WebGUI instance.

## Manual Build Image:
  * change directory to DockerFile of docker-omnibus-8.1
  * docker build -t omnibus-8.1 .
  * change directory to DockerFile of docker-webgui-8.1
  * docker build -t noi-webgui-8.1 .

## Manual run containers:
  * docker run --name omnibus -d -e OBJSRV=AGG -p 4100:4100 omnibus-8.1
  * docker run --name webgui -d --link omnibus:agg -e OBJECTSERVER_PRIMARY_HOST=agg -e OBJECTSERVER_PRIMARY_PORT=4100 -e OBJECTSERVER_PRIMARY_NAME=AGG -e OBJECTSERVER_USER=root -e OBJECTSERVER_PASSWORD= -p 16311:16311 noi-webgui-8.1

### Object Server
Objectserver runs as dedicated user netcool and contains a single volume /db for database. Database initialization is called on container startup only.

Container can be configured by following environment variables
  * OBJSRV\_PORT (default 4100) - port to start Object Server
  * DBINIT\_EXTRA - extra arguments passwd to nco\_dbinit
  * OBJSRV\_EXTRA - extra arguments passed to nco\_objserv
  * OMNIDAT\_EXTRA - extra content inserted to omni.dat

### WebGUI

WebGUI can be configured using following environment variables, that will be modified in *ncwDataSourceDefinitions.xml* configuration file

  * OBJECTSERVER\_PRIMARY\_HOST 
  * OBJECTSERVER\_PRIMARY\_NAME 
  * OBJECTSERVER\_USER 
  * OBJECTSERVER\_PASSWORD 
  * OBJECTSERVER\_ENCRYPTED 
  * OBJECTSERVER\_ALGORITHM\_ATTRIBUTE 
  * OBJECTSERVER\_SSL 
  * OBJECTSERVER\_PRIMARY\_PORT 
  * OBJECTSERVER\_SECONDARY\_HOST 
  * OBJECTSERVER\_FAILOVER 
  * OBJECTSERVER\_SECONDARY\_PORT



