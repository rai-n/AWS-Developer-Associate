* EBS - Elastic block store volume is a network drive you can attach at your instance while they run
* it allow instance to persist data even after their termination 
* They can only be mounted to one instance at a time (CCP level)
* They are bounded to a specific availability zone 
* Like a network usb 
* Free tier: 30gb of free EBS storage as General purpose SSD or Magnetic per month

* EBS is a network drive. It uses network to communicate to instance, so could have latency
* It can be detached from an EC2 instance and attached to another one quickly
* It's locked to an Availability Zone, to move it you need to first snapshot it 

* You have to provision the size of the volume before hand like GB and IOPS and pay for provisioned capacity
* "Delete on termination" attribute. By default the root EBS volume is deleted. By default, any other attached BS volumes is not deleted.