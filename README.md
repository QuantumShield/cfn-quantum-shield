# CFN Quantum Shield

Repository for Quantum Shield Quick Start CloudFormation templates.

## Standalone - `templates/standalone.template.yaml`

The standalone quickstart template is built for dropping a single Quantum Shield instance in your public subnet and having it terminate Quantum-Safe TLS connections and proxy requests to a pre-existing load balancer in your VPC.

### Deployment Steps

To deploy the template, you with need to know the VPC id and the subnet id for a public subnet to deploy Quantum Shield in.

1. Create a new CloudFormation stack uploading the `standalone.template.yaml` template setting the VpcId and PublicSubnetId parameters. If you already know what upstream endpoint you would like to proxy requests for (like a pre-existing ALB or NLB), you can also set the UpstreamEndpoint parameter to that DNS.
2. Once the stack is created, the public IP for the Elastic IP created and allocated to Quantum Shield will be added to the stack outputs. Point the domain you would like to configure at that IP.
3. Update the CloudFormation stack without updating the template, setting the FQDN to the domain you've configured to point at Quantum Shield's public IP and setting UpstreamEndpoint to the endpoint you want to proxy to (i.e. http://my-existing-alb.elb.amazonaws.com).
4. Wait for 5 minutes after the stack updates and the changes should be reflected by Quantum Shield.

There is also the option to specify the allocation id of an existing Elastic IP if you already have one created with a domain configured to point at it. You can follow the same steps as above but also setting the ElasticIPAllocationId parameter to the allocation id of the Elastic IP.
