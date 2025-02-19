manage_transit_gateway
==================

A role to create/delete a Transit Gateway with VPC/VPN attachments.

Requirements
------------

AWS User Account with the following permission:

* ec2:CreateTransitGateway
* ec2:DescribeTransitGateway
* ec2:DeleteTransitGateway
* ec2:CreateTransitGatewayVpcAttachment
* ec2:DescribeTransitGatewayVpcAttachment
* ec2:DeleteTransitGatewayVpcAttachment
* ec2:CreateVpnConnection
* ec2:DescribeVpnConnection
* ec2:DeleteVpnConnection

Role Variables
--------------

* **action** (str): Whether to create or delete the transit gateway. Choices: 'create', 'delete'.
* **transit_gateway** (dict): A dict of parameters needed to create transit gateway.
    **asn** (int): A private Autonomous System Number (ASN) for the Amazon side of a BGP session.
    **tags** (dict): A dict of tags for the transit gateway.
    **description** (str): Description for the transit gateway.
* **vpc_attachment** (list(dict)): A list of dict of parameters to create vpc attachments.
    **name** (str): Name for the VPC attachment.
    **tags** (dict): A dict of tags for the attachment.
    **subnets** (list(str)): A list of subnets to be added to the attachment.
* **vpn_attachment** list(dict): A list of dict of parameters to create vpn attachments.
    **customer_gateway_id** (str): Id of the customer gateway.
* **vpc_route_table** (list(dict)): Route table entries for the VPC.
    **vpc_id** (str): VPC id for which the route should be added..
    **cidr_block** (str): Destination CIDR block.
    **tags** (dict): A dict of tags for the route table.

Dependencies
------------

- role: [aws_setup_credentials](../aws_setup_credentials/README.md)

Example Playbook
----------------
**Create a transit gateway with 2 VPC attachments**

- hosts: localhost
  gather_facts: false
  tasks:
    - name: Create transit gateway
      ansible.builtin.include_role:
        name: cloud.aws_ops.manage_transit_gateway
      vars:
        action: "create"
        transit_gateway:
            asn: 4200000000
            description: "TGW for Cloud team"
            tags:
              "team": "cloud"
        vpc_attachment:
            - name: "vpc-attachment-001"
              tags:
                "team": "cloud"
              subnets:
                - "subnet-xxxx001"
            - name: "vpc-attachment-002"
              tags:
                "team": "cloud"
              subnets:
                - "subnet-xxxx002"


**Create a transit gateway with  VPN attachment**

- hosts: localhost
  gather_facts: false
  tasks:
    - name: Create transit gateway
      ansible.builtin.include_role:
        name: cloud.aws_ops.manage_transit_gateway
      vars:
        action: "create"
        transit_gateway:
          asn: 4200000000
          description: "TGW for Cloud team"
          tags:
            "team": "cloud"
        vpn_attachment:
          - customer_gateway_id: "cgw-01b56884848a25446"

License
-------

GNU General Public License v3.0 or later

See [LICENCE](https://github.com/ansible-collections/cloud.aws_ops/blob/main/LICENSE) to see the full text.

Author Information
------------------

- Ansible Cloud Content Team
