---

copyright:
  years: 2017, 2020
lastupdated: "2020-02-18"

keywords: terraform provider plugin, terraform classic infrastructure, terraform classic, terraform softlayer, terraform sl, terraform vsi, terraform bare metal server

subcollection: terraform

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:preview: .preview}
{:external: target="_blank" .external}

# Classic infrastructure data sources
{: #infrastructure-data-sources}

Review the data sources that you can use to retrieve information about your classic {{site.data.keyword.cloud_notm}} infrastructure resources. All data sources are imported as read-only information. You can reference the output parameters for each data source by using Terraform interpolation syntax.

## `ibm_compute_bare_metal`
{: #classic-bare-metal}

Retrieve information about a classic {{site.data.keyword.cloud_notm}} bare metal server.
{: shortdesc}

### Sample Terraform code
{: #classic-bare-metal-sample}

```hcl
data "ibm_compute_bare_metal" "bare_metal" {
  hostname    = "jumpbox"
  domain      = "mydomain.com"
  most_recent = true
}

data "ibm_compute_bare_metal" "bare_metal" {
  global_identifier = "a111a1a1-11a1-11a1-aa1a-11aaa111a1aa"
}
```

### Input parameters
{: #classic-bare-metal-input}

Review the input parameters that you can specify for your data source.
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|----------------------|
|`hostname`|String|Optional|The hostname of the bare metal server. If you specify the `hostname`, do not specify `global_identifier` at the same time. |
|`domain`|String|Optional|The domain of the bare metal server. If you specify this option, do not specify `global_identifier` at the same time. |
|`global_identifier`|String|Optional|The unique global identifier of the bare metal server. To see global identifier, log in to the [IBM Cloud Classic Infrastructure (SoftLayer) API](https://api.softlayer.com/rest/v3.1/SoftLayer_Account/getHardware.json), using your API key as the password. If you specify this option, do not specify `hostname`, `domain`, or `most_recent` at the same time.|
|`most_recent`|Boolean|Optional|If there are multiple bare metal services, you can set this argument to `true` to import only the most recently created server. If you specify this option, do not specify `global_identifier` at the same time.|
{: caption="Table. Available input parameters" caption-side="top"}

### Output parameters
{: #classic-bare-metal-output}

Review the output parameters that you can access after you retrieved your data source.

|Name|Data type|Description|
|----|-----------|---------|
|`id`|String|The unique identifier of the bare metal server.|
|`datacenter`|String|The data center in which the bare metal server is deployed.|
|`network_speed`|String|The connection speed, expressed in Mbps, for the server network components.|
|`public_bandwidth`|String|The amount of public network traffic, allowed per month.|
|`public_ipv4_address`|String|The public IPv4 address of the bare metal server.|
|`public_ipv4_address_id`|String|The unique identifier for the public IPv4 address of the bare metal server.|
|`private_ipv4_address`|String|The private IPv4 address of the bare metal server.|
|`private_ipv4_address_id`|String|The unique identifier for the private IPv4 address of the bare metal server.|
|`public_vlan_id`|String|The public VLAN used for the public network interface of the server. |
|`private_vlan_id`|String|The private VLAN used for the private network interface of the server. |
|`public_subnet`|String|The public subnet used for the public network interface of the server. |
|`private_subnet`|String|The private subnet used for the private network interface of the server. |
|`hourly_billing`|String| The billing type of the server.|
|`private_network_only`|String|Specifies whether the server only has access to the private network.|
|`user_metadata`|String|Arbitrary data available to the computing server.|
|`notes`| String|Notes associated with the server.|
|`memory`|Integer|The amount of memory in gigabytes, for the server.|
|`redundant_power_supply`| Boolean|When the value is `true`, it indicates additional power supply is provided.|
|`redundant_network`|Boolean|When the value is `true`, two physical network interfaces are provided with a bonding configuration.|
|`unbonded_network`|Boolean|When the value is `true`, two physical network interfaces are provided without a bonding configuration.|
|`os_reference_code`|String|An operating system reference code that provisioned the computing server.*  
|`tags`|List of Strings|Tags associated with this bare metal server.|
|`block_storage_ids`|List of Strings|Block storage to which this computing server have access.|
|`file_storage_ids`|List of Strings|File storage to which this computing server have access.|
|`ipv6_enabled`|Boolean|Indicates whether the public IPv6 address is enabled or not.|
|`ipv6_address`|String|The public IPv6 address of the bare metal server.|
|`ipv6_address_id`|String|The unique identifier for the public IPv6 address of the bare metal server.|
|`secondary_ip_count`|Integer|The number of secondary IPv4 addresses of the bare metal server.|
|`secondary_ip_addresses`|String|The public secondary IPv4 addresses of the bare metal server.|
{: caption="Table 1. Available output parameters" caption-side="top"}

## `ibm_compute_image_template`
{: #classic-image}

Retrieve information about an image template that you can use for a classic bare metal or virtual machine. 
{: shortdesc}

### Sample Terraform code
{: #classic-image-sample}

The following example shows how you can retrieve the ID of an image template and reference this ID in your `ibm_compute_vm_instance` resource.  
{: shortdesc}

```hcl
data "ibm_compute_image_template" "img_tpl" {
    name = "myimage"
}

resource "ibm_compute_vm_instance" "vm1" {
    ...
    image_id = "${data.ibm_compute_image_template.img_tpl.id}"
    ...
}
```

### Input parameters
{: #classic-image-input}

Review the input parameters that you can specify for your data source.
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|----------------------|
|`name`|String|Required|The name of the image template. You can find the name in the [IBM Cloud infrastructure customer portal](https://cloud.ibm.com/classic) by navigating to **Devices > Manage > Images**.|
{: caption="Table. Available input parameters" caption-side="top"}

### Output parameters
{: #classic-image-output}

Review the output parameters that you can access after you retrieved your data source.

|Name|Data type|Description|
|----|-----------|---------|
|`id`|The unique identifier of the image template.|
{: caption="Table 1. Available output parameters" caption-side="top"}


## `ibm_compute_placement_group`
{: #classic-placement-group}

Retrieve information about a placement group. 
{: shortdesc}

### Sample Terraform code
{: #classic-placement-group-sample}

```hcl
data "ibm_compute_placement_group" "group" {
    name = "mygroup"
}
```

### Input parameters
{: #classic-placement-group-input}

Review the input parameters that you can specify for your data source.
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|----------------------|
|`name`|String|Required|The name of the placement group.|
{: caption="Table. Available input parameters" caption-side="top"}

### Output parameters
{: #classic-placement-group-output}

Review the output parameters that you can access after you retrieved your data source.

|Name|Data type|Description|
|----|-----------|---------|
|`id`|String|The unique identifier of the placement group.|
|`datacenter`|String |The data center in which placement group resides.|
|`pod`|String|The pod in which placement group resides.|
|`rule`|String|The rule of the placement group.|
|`virtual_guests`|List of objects|A nested block describing the VSIs attached to the placement group. |
|`virtual_guests.id`|String|The ID of the virtual guest.  |
|`virtual_guests.domain`|String|The domain of the virtual guest.  |
|`virtual_guests.hostname`|String|The hostname of the virtual guest.|
{: caption="Table 1. Available output parameters" caption-side="top"}

## `ibm_compute_ssh_key`
{: #classic-ssh-key}

Retrieve information about an SSH key. 
{: shortdesc}

### Sample Terraform code
{: #classic-ssh-key-sample}

```hcl
data "ibm_compute_ssh_key" "public_key" {
    label = "mykey"
}
```

### Input parameters
{: #classic-ssh-key-input}

Review the input parameters that you can specify for your data source.
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|----------------------|
|`label`|String|Required|The label of the SSH key.|
|`most_recent`|Boolean|Optional|If more than one SSH key matches the label, you can set this argument to `true` to import only the most recent key. **NOTE**: The search must return only one match. More or less than one match causes Terraform to fail. Ensure that your label is specific enough to return a single SSH key only, or use the `most_recent` argument.|
{: caption="Table. Available input parameters" caption-side="top"}

### Output parameters
{: #classic-ssh-key-output}

Review the output parameters that you can access after you retrieved your data source.

|Name|Data type|Description|
|----|-----------|---------|
|`id`|String|The unique identifier of the SSH key.  |
|`fingerprint`|String|The sequence of bytes to authenticate or look up a longer SSH key.|
|`public_key`|String|The public key contents.|
|`notes`|String|The notes that are stored with the SSH key.|
{: caption="Table 1. Available output parameters" caption-side="top"}

## `ibm_compute_vm_instance`
{: #classic-vm}

Retrieve information about an existing virtual machine. 
{: shortdesc}

### Sample Terraform code
{: #classic-vm-sample}

```hcl
data "ibm_compute_vm_instance" "vm_instance" {
  hostname    = "myhost"
  domain      = "mydomain.com"
  most_recent = true
}
```

### Input parameters
{: #classic-vm-input}

Review the input parameters that you can specify for your data source.
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|----------------------|
|`hostname`|String|Required| The hostname of the VM instance.|
|`domain`|String|Required|The domain of the VM instance.|
|`most_recent`|Boolean|Optional|If there are multiple VM instances, you can set this argument to `true` to import only the most recently created instance.|
{: caption="Table. Available input parameters" caption-side="top"}

### Output parameters
{: #classic-vm-output}

Review the output parameters that you can access after you retrieved your data source.

|Name|Data type|Description|
|----|-----------|---------|
|`id`|String|The unique identifier of the VM instance.|
|`datacenter`|String|The data center in which the VM instance is deployed.|
|`public_interface_id`|String|The ID of the primary public interface.|
|`private_interface_id`|String|The ID of the primary private interface.|
|`cores`|Integer|The number of CPU cores.|
|`status`|String|The VSI status.|
|`last_known_power_state`|String|The last known power state of a VM instance, in the event the instance is turned off outside the information management system (IMS) or has gone offline.|
|`power_state`|String|The current power state of a VM instance.|
|`ipv4_address`|String|The public IPv4 address of the VM instance.|
|`ip_address_id_private`|String|The unique identifier for the private IPv4 address assigned to the VM instance.|
|`ipv4_address_private`|String|The private IPv4 address of the VM instance.|
|`ip_address_id`|String|The unique identifier for the public IPv4 address assigned to the VM instance.|
|`ipv6_address`|String|The public IPv6 address of the VM instance provided when `ipv6_enabled` is set to `true`.|
|`ipv6_address_id`|String|The unique identifier for the public IPv6 address assigned to the VM instance provided when `ipv6_enabled` is set to `true`.|
|`private_subnet_id`|String|The unique identifier of the subnet `ipv4_address_private` belongs to.|
|`public_ipv6_subnet`|String|The public IPv6 subnet provided when `ipv6_enabled` is set to `true`.|
|`public_ipv6_subnet_id`|String|The unique identifier of the subnet `ipv6_address` belongs to.|
|`public_subnet_id`|String|The unique identifier of the subnet `ipv4_address` belongs to.|
|`secondary_ip_addresses`|String|The public secondary IPv4 addresses of the VM instance.|
|`secondary_ip_count`|Integer|Number of secondary public IPv4 addresses.|
{: caption="Table 1. Available output parameters" caption-side="top"}


## `ibm_dns_domain`
{: #classic-domain}

Retrieve information about a domain. 
{: shortdesc}

### Sample Terraform code
{: #classic-domain-sample}

```hcl
data "ibm_dns_domain" "domain_id" {
    name = "mydomain.com"
}
```

### Input parameters
{: #classic-domain-input}

Review the input parameters that you can specify for your data source.
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|----------------------|
|`name`|String|Required| The name of the domain.|
{: caption="Table. Available input parameters" caption-side="top"}

### Output parameters
{: #classic-domain-output}

Review the output parameters that you can access after you retrieved your data source.

|Name|Data type|Description|
|----|-----------|---------|
|`id`|String|The unique identifier of the domain.|
{: caption="Table 1. Available output parameters" caption-side="top"}

## `ibm_dns_domain_registration`
{: classic-domain-reg}

Retrieve information about a domain registration from the IBM DNS Domain Registration Service. The domain must initially be registered via the UI of the IBM Cloud DNS Registration Service. Typically the Domain Registration data source is used in configuration with global load balancing services, like for example Cloudflare, Akamai or IBM Cloud Internet Services (Cloudflare). For additional usage instructions see the resource `ibm_dns_domain_registration_nameservers`. 

### Sample Terraform code
{: classic-domain-reg-sample}

```hcl
data "ibm_dns_domain_registration" "dnstestdomain" {
    name = "mytestdomain.com"
}
```

### Input parameters
{: classic-domain-reg-input}

Review the input parameters that you can specify for your data source.
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|----------------------|
|`name`|String|Required|The name of the DNS domain registration as it was defined in IBM Cloud Infrastructure DNS Registration Service (SoftLayer).|
{: caption="Table. Available input parameters" caption-side="top"}

### Output parameters
{: classic-domain-reg-output}

Review the output parameters that you can access after you retrieved your data source.

|Name|Data type|Description|
|----|-----------|---------|
|`id`|String|The unique identifier of the domain.|
{: caption="Table 1. Available output parameters" caption-side="top"}

## ibm_dns_secondary
{: #classic-dns-secondary}

Retrieve information about a DNS secondary zone. 
{: shortdesc}

### Sample Terraform code
{: #classic-dns-secondary-sample}

```hcl
data "ibm_dns_secondary" "secondary_id" {
    name = "test-secondary.com"
}
```

### Input parameters
{: #classic-dns-secondary-input}

Review the input parameters that you can specify for your data source.
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|----------------------|
|`zone_name`|String|Required|The name of the secondary zone.|
{: caption="Table. Available input parameters" caption-side="top"}

### Output parameters
{: #classic-dns-secondary-output}

Review the output parameters that you can access after you retrieved your data source.

|Name|Data type|Description|
|----|-----------|---------|
|`id`|String|The unique identifier of the secondary.|
|`transfer_frequency`|Integer|Signifies how often a secondary DNS zone transferred in minutes.|
|`master_ip_address`|String|The IP address of the master name server where a secondary DNS zone is transferred from.|
|`status_id`|String|The current status of a secondary DNS record.|
|`status_text`|String|The textual representation of a secondary DNS zone's status.|
{: caption="Table 1. Available output parameters" caption-side="top"}

## `ibm_lbaas`
{: #classic-lbaas}

Retrieve information about a classic {{site.data.keyword.cloud_notm}} load balancer. 
{: shortdesc}
 
### Sample Terraform code
{: #classic-lbaas-sample}

```hcl
resource "ibm_lbaas" "lbaas" {
  name        = "test"
  description = "updated desc-used for terraform uat"
  subnets     = [1878778]
  datacenter  = "dal09"

  protocols = [{
    "frontend_protocol" = "HTTP"
    "frontend_port" = 80
    "backend_protocol" = "HTTP"
    "backend_port" = 80
    "load_balancing_method" = "round_robin"
  }]

  server_instances = [{
    "private_ip_address" = "10.1.11.11",
  },
  ]
}
    data "ibm_lbaas" "tfacc_lbaas" {
    name = "test"
}

```

### Input parameters
{: #classic-lbaas-input}

Review the input parameters that you can specify for your data source.
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|----------------------|
|`name`|String|Required|The name of the load balancer.|
{: caption="Table. Available input parameters" caption-side="top"}

### Output parameters
{: #classic-lbaas-output}

Review the output parameters that you can access after you retrieved your data source.

|Name|Data type|Description|
|----|-----------|---------|
|`description`|String|A description of the load balancer.|
|`datacenter`|String|The data center where load balancer is located.|
|`protocols`|List of objects|A nested block describing the protocols assigned to the load balancer.   |
|`protocols.frontend_protocol`|String|The front-end protocol.  |
|`protocols.frontend_port`|Integer|The front-end protocol port number.  |
|`protocols.backend_protocol`|String|The backend protocol.  |
|`protocols.backend_port`|Integer|The backend protocol port number.  |
|`protocols.load_balancing_method`|String|The load balancing algorithm.  |
|`protocols.session_stickiness`|Boolean|Session stickiness.  |
|`protocols.max_conn`|Integer|The number of connections the listener can accept.  |
|`protocols.tls_certificate_id`|String|The ID of the SSL/TLS certificate being used for a protocol.  |
|`protocol_id`|String|The UUID of a load balancer protocol.|
|`server_instances`|List of objects|A nested block describing the server instances for this load balancer.  |
|`server_instances.private_ip_address`|String|The private IP address of a load balancer member.  |
|`server_instances.weight`|String|The weight of a load balancer member.  |
|`server_instances.status`|String|Specifies the status of a load balancer member as `UP` or `DOWN`.  |
|`server_instances.member_id`|String|The UUID of a load balancer member.|
|`health_monitors`|List of objects|A nested block describing the health_monitors assigned to the load balancer. |
|`health_monitors.protocol`|String|The backend protocol.  |
|`health_monitors.port`|String| The backend port.  |
|`health_monitors.interval`|Integer|The interval in seconds to perform the health check.    |
|`health_monitors.max_retries`|Integer| The maximum retries before the load balancer is considered unhealthy.  |
|`health_monitor.timeout`|String| The health check method.  |
|`health_monitor.url_path`|String| If monitor is "HTTP", this specifies URL path.  |
|`health_monitor.monitor_id`|String| The health monitor UUID.|
|`type`|String|Specifies whether a load balancer is public or private.|
|`status`|String|Specifies the operation status of the load balancer as 'ONLINE' or 'OFFLINE'.|
|`vip`|String|The virtual IP address of the load balancer.|
|`server_instances_up`|Integer|The number of service instances which are in the `UP` health state.|
|`server_instances_down`|Integer|The number of service instances which are in the `DOWN` health state.|
|`active_connections`|Integer|The number of total established connections.|
|`use_system_public_ip_pool`|String|It specifies whether the public IP addresses are allocated from system public IP pool or public subnet from the account ordering the load balancer.|
|`ssl_ciphers`|Array|The list of ssl offloads.|
{: caption="Table 1. Available output parameters" caption-side="top"}

## `ibm_network_vlan`
{: #classic-vlan}

Retrieve information about a VLAN. 
{: shortdesc}

### Sample Terraform code
{: #classic-vlan-sample}

```hcl
data "ibm_network_vlan" "vlan_foo" {
    name = "myvlan"
}
```

### Input parameters
{: #classic-vlan-input}

Review the input parameters that you can specify for your data source.
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|----------------------|
|`name`|String|Conditional|The name of the VLAN. This values is required if neither the VLAN number nor the router host name are specified. To retrieve the name, go to the [IBM Cloud infrastructure portal](https://cloud.ibm.com/classic/network/vlans) and navigate to **Network > IP Management > VLANs**.|
|`number`|Integer|Conditional|The VLAN number. This value is required if no VLAN name is provided. To find the number, go to the [IBM Cloud infrastructure portal](https://cloud.ibm.com/classic/network/vlans).|
|`router_hostname`|String|Conditional| The host name of the primary VLAN router. This value is required if no VLAN name is provided. To find the host name, go to the [IBM Cloud infrastructure portal](https://cloud.ibm.com/classic/network/vlans).|
{: caption="Table. Available input parameters" caption-side="top"}

### Output parameters
{: #classic-vlan-output}

Review the output parameters that you can access after you retrieved your data source.

|Name|Data type|Description|
|----|-----------|---------|
|`id`|String|The unique identifier of the VLAN.|
|`subnets`|List of objects|The collection of subnets associated with the VLAN.    |
|`subnets.id`|String|The ID of the subnet.      |
|`subnet.subnet`|String|The subnet for the vlan.    |
|`subnet.subnet-type`|String|A subnet can be one of several types. `PRIMARY, ADDITIONAL_PRIMARY, SECONDARY, ROUTED_TO_VLAN, SECONDARY_ON_VLAN, STORAGE_NETWORK, and STATIC_IP_ROUTED`. A `PRIMARY` subnet is the primary network bound to a VLAN within the IBM Cloud network. An `ADDITIONAL_PRIMARY` subnet is bound to a network VLAN to augment the pool of available primary IP addresses that may be assigned to a server. A `SECONDARY` subnet is any of the secondary subnet's bound to a VLAN interface. A `ROUTED_TO_VLAN` subnet is a portable subnet that can be routed to any server on a vlan. A `SECONDARY_ON_VLAN` subnet also doesn't exist as a VLAN interface, but is routed directly to a VLAN instead of a single IP address.    |
|`subnet.subnet-size`|String|The size of the subnet for the VLAN.    |
|`subnet.gateway`|String|A subnet's gateway address.    |
|`subnet.cidr`|Integer| A subnet's Classless Inter-Domain Routing prefix. This is a number between 0 and 32 signifying the number of bits in a subnet's netmask. |
|`virtual_guests`|List of objects|A nested block describing the VSIs attached to the VLAN. |
|`virtual_guests.id`|String|The ID of the virtual guest.  |
|`virtual_guests.domain`|String|The domain of the virtual guest.  |
|`virtual_guests.hostname`|String|The hostname of the virtual guest.|
{: caption="Table 1. Available output parameters" caption-side="top"}

## `ibm_security_group`
{: #classic-security-group}

Retrieve information about a security group. 
{: shortdesc}


### Sample Terraform code
{: #classic-security-group-sample}

The following example retrieves information about the `allow_ssh` security group.
{: shortdesc}

```hcl
data "ibm_security_group" "allow_ssh" {
    name = "allow_ssh"
}
```

### Input parameters
{: #classic-security-group-input}

Review the input parameters that you can specify for your data source.
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|----------------------|
|`name`|String|Required|The name of the security group.|
|`description`|String|Optional|The description of the security group.|
|`most_recent`|Boolean|Optional|If more than one security group has the same name or description, you can set this argument to `true` to import only the most recent security group. **NOTE**: The search must return only one match, otherwise Terraform fails. Ensure that your name and description combinations are specific enough to return a single security group key only, or set the `most_recent` argument to `true`.|
{: caption="Table. Available input parameters" caption-side="top"}

### Output parameters
{: #classic-security-group-output}

Review the output parameters that you can access after you retrieved your data source.

|Name|Data type|Description|
|----|-----------|---------|
|`id`|String|The unique identifier of the security group.|
|`description`|String|The description of the security group.|
{: caption="Table 1. Available output parameters" caption-side="top"}
