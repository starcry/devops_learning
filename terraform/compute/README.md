assignment

infrastructure
* 1 RDS database
* 1 Application Loadbalancer
* codecommit or some other private repo
* 1 auto scaler
** 3 instances
** user data in autoscaler
*** pulls down webserver code
*** deploys code to webserver
*** starts web server

TODO
use packer to build the ami for the webserver
* use the standard amazon ami
* install any webserver of your choice
* any other apps you may need

public subnet
* application load balancer with autoscalar attached
* request hits the load balancer
* then forwarded to 1 of the instances
* when a request hits the instances
* a request is fired to the database
* data from the database is displayed

Private subnet
* RDS database
* serves data
