# k3s

This is a role to install K3S on one or more hosts. It is a modified version of the upstream K3S role. It downloads the K3S binary from GitHub releases, so this should work on any host that can run K3S. Tested on CentOS 7, 8 Stream and 9 Stream.  

This role consolidates the upstream K3S worker role and the K3S master role into one role. It uses the currently targeted hosts as cluster members.

## Requirements

- Hosts capable of running K3S. 

## Variables
| Variable | Default | Description |
|----------|---------|-------------|
| `k3s_version` | `"v1.26.1+k3s1"` | The version of K3S to install. |
| `k3s_server_location` | `"/var/lib/rancher/k3s"` | The location of the K3S data directory. |
| `k3s_systemd_dir` | `"/etc/systemd/system"` | The location of the systemd directory. |
| `k3s_retrieve_kubeconfig` | `true` | Whether to retrieve the kubeconfig from the master node. |
| `k3s_kubeconfig_local_dest` | `"./kubeconfig.yaml"` | Where to download the kubeconfig to. Skip with `--skip-tags=get-config`. |
| `k3s_extra_server_args` | `""` | Extra arguments to pass to the K3S server. |
| `k3s_role` | `"master"` | The role of the host. Can be `master` or `worker`. At least one host needs to be `master`! |
| `k3s_master_ip` | `""` | The IP address of the master node. If unset will be gotten from the first host with the `master` role. |

## Configuration

This role will install a K3S cluster on one or multiple hosts.  
The `k3s_role` variable must be set to `master` on at least one host.  
The `k3s_master_ip` variable can be set to manually set the IP address of the master node that should be used to connect to the cluster.  
If this variable is not set, the first host with the `master` role will be used.  


### Example

The following is an example of a playbook that uses this role.  

<details> <summary> <code> inventory.ini </code> </summary>

```ini
[k3s]
server1
server2
server3
```

</details>

<details> <summary> <code> group_vars/all.yml </code> </summary>

```yaml
k3s_version: "v1.24.8+k3s1"
k3s_server_location: /var/lib/rancher/k3s
k3s_systemd_dir: /etc/systemd/system
k3s_kubeconfig_local_dest: ./kubeconfig.yaml
k3s_extra_server_args: ""
```

</details>


<details> <summary> <code> host_vars/server1.yml </code> </summary>

```yaml
k3s_role: master

```

</details>

<details> <summary> <code> playbook.yml </code> </summary>

```yaml
- hosts: k3s
  roles:
    - k3s_cluster
```

</details>
