# Weatherapp

The weather app shows the upcoming weather to the users.

## Prerequisites

* An [openweathermap](http://openweathermap.org/) API key.

### Docker

The frontend and the backend of the application contains a **Dockerfile** that can be used to build images that can be run on any environment.

Before running thr application, please get an api key and update it in weatherapp-master/backend/.env file.

For building and running the backend, the following commands can be used:

`cd weatherapp-master/backend`

`docker build -t weatherapp_backend . && docker run --rm -i -p 9000:9000 --name weatherapp_backend -t weatherapp_backend`

For building and running the frontend, the following command can be used:

`cd weatherapp-master/frontend`

`docker build -t weatherapp_frontend . && docker run --rm -i -p 8000:8000 --name weatherapp_frontend -t weatherapp_frontend`

### Docker Compose

The root directory of the application containes a **docker-compose.yml** that connects the frontend and the backend, enabling running the app in a connected set of containers.

To run the application using docker compose, the following commands can be used:

`cd weatherapp-master`

`docker-compose up`

To access the application on your browser the following url can be used:

`localhost:8000`


### Cloud

The application has been deployed on GCP as a Kubernetes deployment. Currently, the frontend and the backend have been exposed as a loadbalancer service. However, we can also use nginx proxy in the future to send the backend requests to the proxy that redirects those requests to the backend to avoid exposing the backend.

To access the application on GCP, the following url can be used:

`http://34.27.38.122:8000/`

### Ansible

The deployment process of the application can be automated using ansible and can be used to deploy the application on any remote ubuntu server. Please remember to fetch and update the api key in the weatherapp-master/backend/env file before proceeding.

The steps for deploying the application on any ubuntu VM from another ubuntu VM are as follows:

- Install ansible

`sudo apt-get update`

`sudo apt-get install software-properties-common`

`sudo apt-add-repository --yes --update ppa:ansible/ansible`

`sudo apt-get install ansible`

-  Test if it's working

`ansible --version`

- First Command

`ansible all -i localhost, --connection=local -m ping`

- To configure ansible for the remote server modify **/etc/ansible/hosts** file with the following entries:

[remote]
remote_test

[remote:vars]
ansible_host=IP_ADDRESS_OF_VIRTUAL_MACHINE
ansible_ssh_private_key_file=~/.ssh/YOUR_SSH_PRIVATE_KEY_FILE
ansible_user=YOUR_USERNAME

Update the remote user's username in app-playbook.yml file.

To deploy docker onto the remote machine, execute the following commands:

`cd weatherapp-master/ansible/`

`ansible-playbook docker-playbook.yml -l remote`

`ansible-playbook app-playbook.yml -l remote`