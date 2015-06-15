Python bindings to the OpenStack Network API
============================================

In order to use the python neutron client directly, you must first obtain an auth token and identify which endpoint you wish to speak to. Once you have done so, you can use the API like so::

    >>> import logging
    >>> from neutronclient.neutron import client
    >>> logging.basicConfig(level=logging.DEBUG)
    >>> neutron = client.Client('2.0', endpoint_url=OS_URL, token=OS_TOKEN)
    >>> neutron.format = 'json'
    >>> network = {'name': 'mynetwork', 'admin_state_up': True}
    >>> neutron.create_network({'network':network})
    >>> networks = neutron.list_networks(name='mynetwork')
    >>> print networks
    >>> network_id = networks['networks'][0]['id']
    >>> neutron.delete_network(network_id)

Python Client API Operations
============================
List network::

	list_networks(retrieve_all=True, \**kwargs)
	Returns a list of networks that tenant has access to.
	Parameters: **retrieve_all** (optional)

Show network::

	show_network(network,\**kwargs)
	Returns data about the network specified
	Parameter : **network** -  the  ID of the network to be shown
	
Create network::

	body = {"network":
	{"name" : "my_network"}
	}
	create_network(body=body)
	Creates a new neutron network with the body specified
	Parameter: **body** – the name and and admin_state_up for the network

Update network::

	body = {"network":
	{"name" : "updated_name"}
	}
	update_network(network, body=body)
	Updates some attributes of the network object
	Parameter : **network** -  the ID of the network that is to be updated
		        **body**    -  specify the updated information here	

Delete network::

	delete_network(network)
	Remove the neutron network and all its associated subnets
	Parameter : **network** -  the  ID of the network to be deleted

List subnets::

	list_subnets(retrieve_all=True, \**kwargs)	
	Returns a list of subnet objects that tenant has access to
	Parameters: **retrieve_all** (optional)

Show subnet::

	show_subnet(subnet, \**kwargs)
	Returns data about the subnet specified
	Parameter : **subnet** -  the  ID of the subnet to be shown
	
Create subnet::

	body = { "subnet": {
    "network_id": "ed2e3c10-2e43-4297-9006-2863a2d1abbc",
    "ip_version": 4,
    "cidr": "10.0.3.0/24",
    "allocation_pools": [{"start": "10.0.3.20", "end": "10.0.3.150"}]
		}
	}
	create_subnet(body=body)
	Creates a new subnet on a specific network. Network_id is mandatory in the body
	Parameter: **body** – parameters to create a new subnet

Delete subnet::

	delete_subnet(subnet)
	Remove the specified subnets
	Parameter : **subnet** -  the  ID of the subnet to be deleted

List ports::

	list_ports(retrieve_all=True, \**kwargs)
	Returns a list of port objects that tenant has access to
	Parameters: **retrieve_all** (optional)
	
Show port::

	show_port(port,\**kwargs)
	Returns the information regarding the port specified in the request.
	Parameters: **port** - the port whose information is required
	
Create port::

	body = {
    "port": {
        "admin_state_up": true,
        "device_id": "d6b4d3a5-c700-476f-b609-1493dd9dadc0",
        "name": "port1",
        "network_id": "6aeaf34a-c482-4bd3-9dc3-7faf36412f12"
		}
	}
	create_port(body=body)
	Creates a new neutron port. Network_id is mandatory in the body
	Parameter: **body** – parameters to create a new port
	
Update port::

	body = {
    "port": {
        "device_id": "37b4f622-5e17-4dca-bf67-7338c5b7dd63"
		}
	}
	update_port(port, body=body)
	Updates some attributes of the port object
	Parameter: **port** -  the ID of the port that is to be updated
			   **body** -  specify the updated information here	

Delete port:

	delete_port(port)
	Removes a port from a neutron network
	Parameter: **port** -  the  ID of the port to be deleted
	
Command-line Tool
=================
In order to use the CLI, you must provide your OpenStack username, password, tenant, and auth endpoint. Use the corresponding configuration options (``--os-username``, ``--os-password``, ``--os-tenant-name``, and ``--os-auth-url``) or set them in environment variables::

    export OS_USERNAME=user
    export OS_PASSWORD=pass
    export OS_TENANT_NAME=tenant
    export OS_AUTH_URL=http://auth.example.com:5000/v2.0

The command line tool will attempt to reauthenticate using your provided credentials for every request. You can override this behavior by manually supplying an auth token using ``--os-url`` and ``--os-auth-token``. You can alternatively set these environment variables::

    export OS_URL=http://neutron.example.org:9696/
    export OS_TOKEN=3bcc3d3a03f44e3d8377f9247b0ad155

If neutron server does not require authentication, besides these two arguments or environment variables (We can use any value as token.), we need manually supply ``--os-auth-strategy`` or set the environment variable::

    export OS_AUTH_STRATEGY=noauth

Once you've configured your authentication parameters, you can run ``neutron -h`` to see a complete listing of available commands.

Release Notes
=============

2.0
-----
* support Neutron API 2.0

2.2.0
-----
* add security group commands
* add Lbaas commands
* allow options put after positional arguments
* add NVP queue and net gateway commands
* add commands for agent management extensions
* add commands for DHCP and L3 agents scheduling
* support XML request format
* support pagination options

2.2.2
-----
* improved support for listing a large number of filtered subnets
* add --endpoint-type and OS_ENDPOINT_TYPE to shell client
* made the publicURL the default endpoint instead of adminURL
* add ability to update security group name (requires 2013.2-Havana or later)
* add flake8 and pbr support for testing and building
