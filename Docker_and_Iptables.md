Docker uses `iptables` to manage its networking rules on the host machine. These rules allow Docker to manage container traffic, including NAT, port forwarding, and inter-container communication. To view the `iptables` rules that Docker has created, you can use the `iptables` command-line utility.

Here’s how you can view the `iptables` rules created by Docker:

### **Step 1: List All `iptables` Rules**

To see all `iptables` rules, including those created by Docker, run the following command:

```bash
sudo iptables -L -n -v
```

- `-L`: Lists all rules.
- `-n`: Displays IP addresses and ports as numbers (instead of resolving them to hostnames and service names).
- `-v`: Provides verbose output, showing packet and byte counters.

This command will list all the `iptables` rules, including those for input, output, and forwarding chains.

### **Step 2: Check the NAT Table**

Docker creates several rules in the `nat` table, especially for things like port forwarding. To view the rules in the `nat` table:

```bash
sudo iptables -t nat -L -n -v
```

This command lists all the rules in the `nat` table, including those that handle network address translation (NAT) for Docker containers.

### **Step 3: Filter Docker-Specific Rules**

Docker typically creates rules in chains with names like `DOCKER` or `DOCKER-USER`. You can filter the output to show only these chains:

```bash
sudo iptables -L DOCKER -n -v
sudo iptables -L DOCKER-USER -n -v
```

- `DOCKER`: Contains rules for container-to-container communication and NAT for published ports.
- `DOCKER-USER`: Allows administrators to add custom rules that affect Docker container traffic.

### **Step 4: View All Chains**

To see all chains, including user-defined chains like `DOCKER` and `DOCKER-USER`, use:

```bash
sudo iptables -S
```

This command outputs all `iptables` rules in a format that shows how they were added.

### **Example of Docker-Related `iptables` Rules**

When you list `iptables` rules, you might see something like this:

```bash
Chain FORWARD (policy DROP)
target     prot opt source               destination
DOCKER-USER  all  --  anywhere             anywhere
DOCKER-ISOLATION-STAGE-1  all  --  anywhere             anywhere
ACCEPT     all  --  anywhere             anywhere             ctstate RELATED,ESTABLISHED
DOCKER     all  --  anywhere             anywhere
ACCEPT     all  --  anywhere             anywhere

Chain DOCKER (2 references)
target     prot opt source               destination
ACCEPT     tcp  --  anywhere             172.17.0.2            tcp dpt:80

Chain DOCKER-USER (1 references)
target     prot opt source               destination
RETURN     all  --  anywhere             anywhere
```

### **Summary**

- **List all `iptables` rules:** `sudo iptables -L -n -v`
- **View `nat` table rules:** `sudo iptables -t nat -L -n -v`
- **Filter Docker-specific chains:** `sudo iptables -L DOCKER -n -v` and `sudo iptables -L DOCKER-USER -n -v`

These commands allow you to inspect how Docker configures the network stack on your host, giving you insight into container communication and port forwarding.
