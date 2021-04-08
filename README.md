# The *infra-challenge* app high availablity deployment

The complete task can be found [here](https://gitlab.com/salamachinas/infra-challenge/-/blob/master/README.md).

## Architecture solution
The HA environment for the application mentioned above consist of 4 virtual machines. First one for reverse proxy (entry point), other three for the application itself.

## Requirements
* Task done with:
  - [Vagrant](https://www.vagrantup.com) and [Ansible](https://www.ansible.com) - local solution;

## Usage
To launch deploy process in the root of this repository run:
```sh
$ vagrant up
```
Once the deployment is complete, you can determine the IP address assigned to the web server:
```sh
$ echo $(vagrant ssh nginx -c "ip address show eth2 | grep 'inet ' |
sed -e 's/^.*inet //' -e 's/\/.*$//'")
```
![show_ip](https://user-images.githubusercontent.com/63558838/114105321-359efd00-98d5-11eb-96f8-650cda9198c3.png)<br/>
Now you can try some tests.
<br/><br/>Unsupported URI:
```sh
$ curl -v -GET http://192.168.1.231
```
![page_not_found](https://user-images.githubusercontent.com/63558838/114105334-3afc4780-98d5-11eb-9de5-4634d8ea6581.png)<br/>
Supported URI:
```sh
$ curl -v -GET http://192.168.1.231/ping
```
![pong](https://user-images.githubusercontent.com/63558838/114105346-42235580-98d5-11eb-892b-a364d4cf3ccb.png)<br/>
After that, you can stop one of the nodes and repeat the test:
```sh
$ vagrant halt node1

$ vagrant global-status

$ curl -v -GET http://192.168.1.231/ping
```
![stopped_one_node](https://user-images.githubusercontent.com/63558838/114105430-6717c880-98d5-11eb-95f2-42f5f1b4a05b.png)<br/>
Try to stop the second one and repeat the test again:
```sh
$ vagrant halt node2

$ vagrant global-status

$ curl -v -GET http://192.168.1.231/ping
```
![stopped_two_nodes](https://user-images.githubusercontent.com/63558838/114105414-5f582400-98d5-11eb-807d-a2e2a784bf27.png)<br/>
Finally stop the last one and check:
```sh
$ vagrant halt node3

$ vagrant global-status

$ curl -v -GET http://192.168.1.231/ping
```
![stopped_all_nodes](https://user-images.githubusercontent.com/63558838/114105424-62ebab00-98d5-11eb-8cc3-5ebfd0423a62.png)<br/>
Now you can launch some nodes again and try to test one more time:
```sh
$ vagrant up node2 node3

$ vagrant global-status

$ curl -v -GET http://192.168.1.231/ping
```
![launch_nodes_again](https://user-images.githubusercontent.com/63558838/114105451-6da64000-98d5-11eb-8f0b-1f25646898ee.png)
