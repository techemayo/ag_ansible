

The repo contains the code for the scenario described ahead:


**Dynamic Configuration File Generation Using Jinja2 Templating with Node Name-Based Logic**

**Scenario**:
You are tasked with deploying a multi-node application cluster. Each node in the cluster requires a configuration file. The content of this configuration file varies based on the node's role, its position in the cluster, and its name.

**Requirements**:

1. **Roles and Node Names**:
- There are three roles: `leader`, `worker`, and `backup`.
- Nodes are named in the format: `nodeX`, where `X` is a number (e.g., `node1`, `node2`, etc.).
- The `leader` node requires additional configuration directives.
- The `worker` nodes have a set of common directives but also need a unique identifier based on their node name.
- The `backup` node requires directives to know which node it backs up.

2. **Configuration Directives**:
- All nodes need the following common directives:
```
cluster_name: "my_cluster"
log_level: "info"
```
- The `leader` node requires an additional directive:
```
role: "leader"
```
- Each `worker` node requires the common directives plus:
```
role: "worker"
worker_id: [unique_id]
```
Where `[unique_id]` is calculated as `1000 + X` (from the node name `nodeX`).
- The `backup` node requires:
```
role: "backup"
backup_for: [node_name]
```
Where `[node_name]` is the name of the node it backs up (could be leader or any worker).

3. **Jinja2 Template**:
- Create a single Jinja2 template that, when provided with the necessary variables (including node name), will generate the appropriate configuration for any node type.
- Use advanced Jinja2 features like conditional statements, loops, filters, and string manipulations to achieve this.

4. **Ansible Playbook**:
- Write an Ansible playbook that uses your Jinja2 template to generate configuration files for a sample cluster setup:
- 1 leader node named `node1`
- 3 worker nodes named `node2`, `node3`, and `node4`
- 1 backup node named `node5` (backing up `node1`)
- The playbook should place these configuration files in a directory named `configs` on the Ansible control node, with each file named `config_[node_name].conf`.

**Expected Deliverables**:
1. The Jinja2 template file.
2. The Ansible playbook.
3. Sample configuration outputs for each node type.

**Hints**:
- Remember to ensure idempotency. If the playbook is run multiple times, it shouldn't produce different results unless the input variables change.
