# 03.Networking

Docker networks are the backbone for communication between containers in a containerized application. They act as virtual networks within the host machine, allowing containers to connect and interact with each other and the outside world in a controlled manner.

## Docker Network Models:

Docker provides various network models to suit different application requirements. Understanding these models and their characteristics is crucial for effective network management. Here's a breakdown with commands for creating and inspecting them:

### Bridge Network (Default):

Creates a virtual network bridge (docker0) on the host machine. This bridge acts as a central hub for containers to connect and communicate using private IP addresses assigned by Docker's internal DHCP server. Containers can access the host's network and the internet (if allowed) through the bridge interface.

## Technical Details

- **Virtual Ethernet Devices (veth pairs):**
  Docker utilizes veth pairs to establish virtual network connections between containers and the bridge (docker0).

- **Bridge Network:**
  Docker manages IP address allocation within the bridge network using a subnet. Each container connected to the bridge network is assigned an IP address from this subnet.

- **Firewall Rules:**
  On the host machine, firewall rules control outbound traffic from containers to the external world. These rules are enforced to regulate and secure network communication originating from the containers.

**Creating a Bridge Network:**
```bash
docker network create web-app-network
```
**Verifying network:**
```bash
docker network ls
```
**Attach a Container:**
```bash
docker run -d --name my-webserver --network bridge my-webserver-image
# Verify container is running
docker ps
```

**Verify Attached Containers:**
```bash
docker network inspect bridge
```
This shows detailed information about the bridge network, including a `"Containers"` section listing attached containers (e.g., `my-webserver`).

**Detach a Container:**
```bash
docker network disconnect bridge my-webserver
docker ps
```
**Rename the Network** (Not recommended for bridge network):
```bash
docker network rename bridge new_network_name
```
**Remove the Network** (Not recommended for bridge network):
```bash
docker network rm new_network_name
```

**Benefits:** Simple to set up, ideal for most container communication needs within a single host.

**Example:** Use a bridge network for a web application with a database container, where they need to communicate internally but also access external resources.


### Host Network:
Removes network isolation between containers and the host machine. Containers share the same IP address and network configuration as the host, allowing direct access to the host's network resources.
## Technical Details

- **Containers Inheritance:**
  Containers inherit the host's IP address, subnet mask, gateway, and DNS settings.

- **Direct Access to Network Resources:**
  Containers can directly access network resources available to the host machine (e.g., physical network devices).


**Create: (No explicit creation needed. host network utilizes the host's network) (NOT RECOMMENDED):**


**Attach a Container:**
```bash
docker run -d --name my-legacy-app --network host my-legacy-app-image
docker ps
```
**Detach a Container:** (Not applicable for the host network)
**Rename/Remove the Network:** (Not applicable for the host network as it utilizes the host's network)

**Inspecting the Network:**
```bash
docker network inspect host
```
**Considerations:** Avoid using the host network for security reasons unless absolutely necessary. It breaks isolation and exposes container processes to the host's network environment.

### Overlay Networks (Advanced):**
Designed for distributed applications running on multiple Docker hosts in a cluster. It creates a virtual overlay network that spans all participating hosts. Containers connected to the overlay network can communicate with each other regardless of their physical location on the host machines.

## Technical Details (Requires additional configuration for overlay drivers)

- **Underlying Network Technologies:**
  Relies on underlying network technologies like VXLAN or GRE to encapsulate container traffic and route it across the cluster.

- **Overlay Network Management:**
  Overlay drivers like Docker Swarm or Kubernetes manage the overlay network creation, configuration, and container attachment.


**Benefits:** Enables communication between containers across different Docker hosts.

**Example:** Use an overlay network for a microservices architecture deployed on multiple Docker hosts.

### None Network:
Provides complete network isolation for containers. Containers connected to the `none` network have no network interface and cannot communicate with each other or the external world.

## Technical Details

- **Network Interface Creation:**
No network interface is created for the container, essentially disabling all network communication.

**Creating a None Network:** (No explicit creation needed. none provides network isolation)
```bash
docker run -d --network none isolated-container-image
```
**Attach a Container:**
```bash
docker run -d --name my-encryption-service --network none isolated-container-image
docker ps
```
**Inspecting the Network:**
```bash
docker inspect my-encryption-service
```
**Benefits:** Useful for specific scenarios where network isolation is critical, such as processing sensitive data.

**Example:** Use a none network for a container that performs data analysis on local files without needing any external network access.

## Best Practices for Docker Networking:

- **Use Custom Networks:** Create custom networks (bridge by default) for your applications to improve isolation and security.
- **Connect Containers Strategically:** Connect containers only to the networks they need to access for their functionality. Minimize unnecessary communication paths. Use commands like `docker network connect <network-name> <container-name>` to manage connections.
- **Expose Ports Only When Needed:** Don't expose container ports unless your application requires external access. This enhances security. Use the `-p` flag during `docker run` to expose ports.
- **Leverage Network Policies:** Utilize Docker network policies (advanced topic) to define fine-grained access controls for your containers.

## Connecting Containers:

Docker provides two main methods for connecting containers to networks:

1. **Connecting Containers During Startup (`docker run`):**

   This is the most common approach when creating a new container. You can specify the network using the `--network` flag during the `docker run` command.
   ```bash
   docker run -d --network web-app-network web-server-image
   ```
2. **Connecting Existing Containers to a Network (`docker network connect`):**
   This method is useful for connecting already running containers to a network.
   ```bash
   docker network connect web-app-network my-existing-container

### Additional Commands:

Here are additional Docker network management commands:

- `docker network create <network-name>`: Creates a new network with the specified name.
- `docker network ls`: Lists all networks on your Docker host.
- `docker network inspect <network-name>`: Provides detailed information about a specific network.
- `docker network prune`: Removes unused networks (use with caution).
- `docker network rename <old-network-name> <new-network-name>`: Renames a network.
- `docker network rm <network-name>`: Removes a network (only if no containers are connected to it).


## Alright, see you in the next module.