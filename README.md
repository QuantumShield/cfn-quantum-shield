# Quantum Shield CloudFormation Templates

Repository for Quantum Shield Quick Start CloudFormation templates.

## Static Deployment - `templates/static.template.yaml`

The static quickstart template is built for dropping a single Quantum Shield instance in your public subnet and having it terminate Quantum-Safe TLS connections and proxy requests to a pre-existing load balancer in your VPC.

### Deployment Steps

To deploy the template, you will need to know the VPC id and the subnet id for a public subnet to deploy Quantum Shield in.

1. Create a new CloudFormation stack uploading the `static.template.yaml` template setting the VpcId and PublicSubnetId parameters. If you already know what upstream endpoint you would like to proxy requests for (like a pre-existing ALB or NLB), you can also set the UpstreamEndpoint parameter to that DNS.
2. Once the stack is created, the public IP for the Elastic IP created and allocated to Quantum Shield will be added to the stack outputs. Point the domain you would like to configure at that IP.
3. Update the CloudFormation stack without updating the template, setting the FQDN to the domain you've configured to point at Quantum Shield's public IP and setting UpstreamEndpoint to the endpoint you want to proxy to (i.e. http://my-existing-alb.elb.amazonaws.com).
4. Wait for 5 minutes after the stack updates and the changes should be reflected by Quantum Shield.

There is also the option to specify the allocation id of an existing Elastic IP if you already have one created with a domain configured to point at it. You can follow the same steps as above but also setting the ElasticIPAllocationId parameter to the allocation id of the Elastic IP.

## Clustered Deployment - `templates/cluster.template.yaml`

The cluster quickstart template is built for initializing a cluster of Quantum Shield instances behind a Network Load Balancer to enable terminating QUantum-Safe TLS connections while also satisfying high-availability concerns.

### Deployment Steps

To deploy the template, you will need to know the VPC id, the public subnet ids for the Network Load Balancer in and the private subnet ids for the Quantum Shield instances and EFS mount targets.

1. Create a new CloudFormation stack uploading the `cluster.template.yaml` template filling in the parameters as necessary.
2. If no Route53 Hosted Zone Id is specified, DNS will need to be configured to point at the NLB created by the CloudFormation stack.
3. Once DNS is configured, manually invoke or wait for the TLSRenewal function to request a publicly trusted certificate from Let's Encrypt and reload Quantum Shield on all deployed instances. The TLSRenewal function can be manually invoked in the Lambda console if the daily scheduled event already executed.

### Architecture Overview

![Quick Start architecture for Quantum Shield in a High Availability deployment](/docs/quantum_shield_cluster.png)
