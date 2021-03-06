Installation
------------
    git clone https://github.com/OpenChemistry/mongochemdeploy.git
    cd mongochemdeploy
    pip install -e .

This install a command line utility called ```ocdeploy``` which is a simple wrapper around ```docker-compose```, allowing
various parts of the stack to be developed from the host machine.

Bringing up the stack
---------------------

To bring up the stack run the following commands:

    ocdeploy pull
    docker pull openchemistry/jupyterlab:latest
    ocdeploy up
    
This will bring the stack in docker containers. The following ports are exposed on the host machine:

- ```8080``` - The Girder server.
- ```8888``` - The single page app.



Bind mounting development repositories from host
------------------------------------------------

In order to update code that is running in the containers it is possible to bind mount repositories that exist on the host
machine. If any of the following variables are set in ```<repo>/dev/dev.env``` the repository will be mounted in to the correct
container.

- ```MONGOCHEMCLIENT``` - The single page app.
- ```MONGOCHEMSERVER``` - The server side Girder plugins.
- ```OPENCHEMISTRYPY``` - The OpenChemistry Python library.
- ```JUPTERLAB_APP_DIR``` - The JupyterLab application directory containing the JupyterLab extensions.

Once ```<repo>/dev/dev.env``` has been update you can bring the stack back up using ```ocdeploy up```.

Bringing down the stack
-----------------------

```ocdeploy down```

Note: That if the ```-v``` option is provide the volumes will also be removed along with your data.


Troubleshooting
---------------

### Cleaning up old JupyterLab containers

Currenting if the user doesn't successfully logout of the single page app any containers associated with their notebooks can hang around, this can cause problems when trying to create new notebook session. The following command can be using to remove the containers.

     docker ps -a  | grep jupyterlab | awk '{print $1}' |xargs -n1  docker rm
