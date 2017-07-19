# devops-tutz
# DevOps Learning Initiative #DevOpsInitiative
This project is a part of devops learning initiave to get all related project
search #devOpsInitiative

### Project List 
* [todobackend](https://github.com/abhishek-jaiswal/todobackend) python app the sample application repository, including the continuous delivery workflow
* [acceptance-test](https://github.com/abhishek-jaiswal/acceptance-test) front end acceptance test Node.js test runner that runs acceptance tests against the todobackend sample application
* [docker-ansible](https://github.com/abhishek-jaiswal/docker-ansible) Docker ansible image for agent service. Ansible playbook runner
* [todobackend-base-image](https://github.com/abhishek-jaiswal/todobackend-base-image) Python base image Docker base image of the todobackend development and release images


### Continious Delivery Workflow

![continious delivery workflow](images/cdr.png "Continious Delivery Workflow")

### 1. Test
```sh
  # to start with our test part of continuous workflow clone all repo above 
  git clone https://github.com/abhishek-jaiswal/docker-ansible.git
  git clone https://github.com/abhishek-jaiswal/acceptance-test.git
  git clone https://github.com/abhishek-jaiswal/todobackend.git
  git clone https://github.com/abhishek-jaiswal/todobackend-base-image.git
  
  # Build the base image ,
  pushd todobackend-base-image && docker build -t abhishekk/todobackend-base . && popd
  
  # build the Ansible image which will use as a discovery agent
  pushd docker-ansible && docker build -t abhishekk/ansible . && popd
  
  # Build the Development Image
  pushd todobackend/docker/dev/ && docker-compose up agent && docker-compose up builder && docker-compose up test && popd
  
```
![Test Environment](images/test_env.png "Test Environment Workflow")





### 2. Build

### 1. Release
```sh  
  # build the Acceptance test image 
  pushd acceptance-test && docker build -t abhishekk/acceptance-test . && popd

  pushd todobackend/docker/release/
  
  # clear any exiting build and clean the environment
  docker-compose kill && docker-compose rm -f
  
  # Build The release environment
  docker-compose up agent 
  docker-compose run --rm app manage.py collectstatic --noinput
  docker-compose run --rm app manage.py migrate --noinput
  docker-compose up test 
  popd
  
```
![Release Environment](images/release_env.png "Release Environment Workflow")
