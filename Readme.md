# OpenStack: Multi-tenants

## Objective 2 :

### Auto Scaling Application : 

Openstack libraries can be used in Python to create connection with Openstack controller. We need to create a YAML file to authenticate
the user. The format of the YAML file is -

```
clouds:
  <user>:
    region_name: <region_name>
    auth:
      username: 
      password: 
      project_name: 
      project_domain_name: 
      user_domain_name: 
      auth_url: http://<openstack_server>/identity/
    log_file: /tmp/openstackclient_admin.log
    log_level: debug
 ```
 
 The YAML file can placed in current directory or  ~/.config/openstack or /etc/openstack. The above details are present in the <user>-openrc.sh file. 

### Initialize a connection :
We can initialize a connection using openstack.connect and provide the user credentials which are defined in the YAML file. 
```
  Example -       
      conn = openstack.connect(cloud='admin')
 ```
      
### Get the Running Instance :
conn.compute.servers() can be used to get the instances currently running.

### Creating a new Instance :
conn.compute.create_server() can be used to create a new instance. 
```
  Example - 
        server = conn.compute.create_server(
              name="name", image_id="image_id", flavor_id="flavor_id",
              networks=[{"uuid": "network_id"}], key_name="key_name")
 ```
              
### CPU utilization of Instance :
"nova diagnostic <instance>" can be used to get current resource utilization by the instance including the CPU utilization. 
  

### Psuedo Code : 
```
create_instance() : 
  num_instances < max_instance:
    create_new_instance 
    update num_instances
    update instance_list
    
check_cpu_util() :
   while(1):
      for instance in instance_list: 
           cpu_util > threshold: 
                create_instance()
      sleep(sometime)
      
instance_list = conn.compute.servers()
num_instance = len(instance_list)
```
  
Objective 4 - Eric
  
  To achiever inter vn connectivity, edit the default security group allow ingress and egress traffic from anywhere to anywhere.
  This creates a major security flaw but achieves the objective.
  
  For specific network mangement security groups can be created and then applied to interfaces. This is done by either editing the vm security group or the specific port security group
  In order to achieve specific connecitivty between VMs, a rule directed at blocking traffic can be applied as a egress rule to one of the vms. This can also be a blanket rule, such as stopping traffic from accessing the internet by blocking egress traffic on all tcp and udp ports. This rule will need seperate rules allowing traffic to and from different ports if the vm is to communicate with anyone at all.
