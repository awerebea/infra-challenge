# Infra-challenge app HA deployment

Full task can be found [here](https://gitlab.com/salamachinas/infra-challenge/-/blob/master/README.md).

## Architecture solution
The high availablity environment for the application mentioned above consist of 4 virtual machines. First one for reverse proxy (entry point), other three for the application itself.

## Requirements
* Task done with:
  - [Vagrant](https://www.vagrantup.com) and [Ansible](https://www.ansible.com) - local solution;

## Usage
To launch deploy process in the root of this repository run:
```sh
$ vagrant up
```
After the deployment is complete, you can try to run some tests.
<br/><br/>Unsupported URI:
```sh
$ curl -v -GET http://10.2.2.100
```
![page_not_found](https://user-images.githubusercontent.com/63558838/114095840-98d56300-98c6-11eb-8c3e-88b87dcbfb82.png)<br/>
Supported URI:
```sh
$ curl -v -GET http://10.2.2.100/ping
```
![pong](https://user-images.githubusercontent.com/63558838/114095847-9b37bd00-98c6-11eb-93f0-e38f1b43966f.png)<br/>
After that, you can stop one of the nodes and repeat the test:
```sh
$ vagrant halt node1

$ vagrant global-status

$ curl -v -GET http://10.2.2.100/ping
```
![stopped_one_node](https://user-images.githubusercontent.com/63558838/114095852-9d018080-98c6-11eb-8593-51ba709e3727.png)<br/>
Try to stop the second one and repeat the test again:
```sh
$ vagrant halt node2

$ vagrant global-status

$ curl -v -GET http://10.2.2.100/ping
```
![stopped_two_nodes](https://user-images.githubusercontent.com/63558838/114095858-9ecb4400-98c6-11eb-91da-4b1a53b3be39.png)<br/>
Finally stop the last one and check:
```sh
$ vagrant halt node3

$ vagrant global-status

$ curl -v -GET http://10.2.2.100/ping
```
![stopped_all_nodes](https://user-images.githubusercontent.com/63558838/114095864-a12d9e00-98c6-11eb-84c1-9927089d3395.png)<br/>
Now you can launch some nodes again and try to test one more time:
```sh
$ vagrant up node2 node3

$ vagrant global-status

$ curl -v -GET http://10.2.2.100/ping
```
![launch_nodes_again](https://user-images.githubusercontent.com/63558838/114095867-a25ecb00-98c6-11eb-80fd-aa71470fbeb1.png)
