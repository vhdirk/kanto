---
title: "Container configuration"
type: docs
description: >
  Customize the deployment of a container instance.
weight: 2
---

### Properties

To control all aspects of the container instance behavior.

| Property | Type | Default | Description |
| - | - | - | - |
| container_name | string | <container_id> | User-defined name for the container, if omitted the internally auto-generated container ID will be set |
| **Image** | | | |
| name | string | | Fully qualified image reference, that follows the {{% refn "https://github.com/opencontainers/image-spec" %}}OCI Image Specification{{% /refn %}}, the format is: `host[:port]/[namespace/]name:tag` |
| **Image - decryption** | | | |
| keys | string[] | | Private keys (GPG private key ring, JWE or PKCS7) used for decrypting the container's image, the format is: `filepath_private_key[:password]` |
| recipients | string[] | | Recipients (only for PKCS7 and must be an x509) used for decrypting the container's image, the format is: `pkcs7:filepath_x509_certificate` |
| **Networking** | | | |
| domain_name | string | <container_name>-domain | Domain name inside the container, if omitted the `container_name` with suffix -domain will be set |
| host_name | string | <container_name>-host | Host name for the container, if omitted the `container_name` with suffix -host will be set |
| network_mode | string | bridge | The container's networking capabilities type based on the desired communication mode, the possible options are: bridge or host |
| extra_hosts | string[] | | Extra host name to IP address mappings added to the container network configuration, the format is: `hostname:ip` |
| **Networking - port mappings** | | | |
| proto | string | tcp | Protocol used for the port mapping from the container to the host, the possible options are: tcp and udp |
| container_port | int | | Port number on the container that is mapped to the host port |
| host_ip | string | 0.0.0.0 | Host IP address |
| host_port | int | | Beginning of the host ports range |
| host_port_end | int | <host_port> | Ending of the host ports range |
| **Host resources - devices** | | | |
| path_on_host | string | | Path to the device on the host |
| path_in_container | string | | Path to the device in the container |
| cgroup_permissions | string | rwm | Cgroup permissions for the device access, possible options are: r(read), w(write), m(mknod) and all combinations are possible |
| privileged | bool | false | Grant root capabilities to all devices on the host system |
| **Host resources - mount points** | | | |
| source | string | | Path to the file or directory on the host that is referred from within the container |
| destination | string | | Path to the file or directory that is mounted inside the container |
| propagation_mode | string | rprivate | Bind propagation for the mount, supported are: rprivate, private, rshared, shared, rslave or slave |
| **Process** | | | |
| env | string[] | | Environment variables that are set into the container |
| cmd | string[] | | Command with arguments that is executed upon the container's start |
| **Resource management** | | | |
| memory | string | | Hard memory limitation of the container as a number with a unit suffix of B, K, M and G, the minimum allowed value is 3M |
| memory_reservation | string | | Soft memory limitation of the container as a number with a unit suffix of B, K, M and G, if `memory` is specified, the `memory_reservation` must be smaller than it |
| memory_swap | string | | Total amount of memory and swap that the container can use as a number with a unit suffix of B, K, M and G, use -1 to allow the container to use unlimited swap |
| **Lifecycle** | | | |
| type | string | unless-stopped | The container's restart policy, the supported types are: always, no, on-failure and unless-stopped |
| maximum_retry_count | int | | Maximum number of retries that are made to restart the container on exit with fail, if the `type` is on-failure |
| retry_timeout | int | | Timeout period in seconds for each retry that is made to restart the container on exit with fail, if the `type` is on-failure |
| **Logging** | | | |
| type | string | json-file | Type in which the logs are produced, the possible options are: json-file or none |
| max_files | int | 2 | Maximum log files before getting rotated |
| max_size | string | 100M | Maximum log file size before getting rotated as a number with a unit suffix of B, K, M and G |
| root_dir | string | <meta_path>/containers/<container_id> | Root directory where the container's log messages are stored |
| mode | string | blocking | Messaging delivery mode from the container to the log driver, the supported modes are: blocking and non-blocking |
| max_buffer_size | string | 1M | Maximum size of the buffered container's log messages in a non-blocking mode as a number with a unit suffix of B, K, M and G |

### Example

The minimal required configuration to spin up an InfluxDB container instance.

```json
{
  "image": {
    "name": "docker.io/library/influxdb:1.8.4"
  }
}
```

### Template

The configuration can be further adjusted according to the use case. The following template illustrates all possible properties with their default values.

{{% warn %}}
Be aware that some combinations may require property removal
{{% /warn %}}

```json
{
    "container_name": "",
    "image": {
        "name": "",
        "decrypt_config": {
            "keys": [],
            "recipients": []
        }
    },
    "domain_name": "",
    "host_name": "",
    "mount_points": [
        {
            "destination": "",
            "source": "",
            "propagation_mode": "rprivate"
        }
    ],
    "config": {
        "env": [],
        "cmd": []
    },
    "host_config": {
        "devices": [
            {
                "path_on_host": "",
                "path_in_container": "",
                "cgroup_permissions": "rwm"
            }
        ],
        "network_mode": "bridge",
        "privileged": false,
        "extra_hosts": [],
        "port_mappings": [
            {
                "proto": "tcp",
                "container_port": 0,
                "host_ip": "0.0.0.0",
                "host_port": 0,
                "host_port_end": 0
            }
        ],
        "resources": {
            "memory": "",
            "memory_reservation": "",
            "memory_swap": ""
        },
        "restart_policy": {
            "type": "unless-stopped",
            "maximum_retry_count": 0,
            "retry_timeout": 0
        },
        "log_config": {
            "driver_config": {
                "type": "json-file",
                "max_files": 2,
                "max_size": "100M",
                "root_dir": ""
            },
            "mode_config": {
                "mode": "blocking",
                "max_buffer_size": "1M"
            }
        }
    }
}
```