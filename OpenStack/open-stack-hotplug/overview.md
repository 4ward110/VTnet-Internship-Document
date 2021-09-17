# CPU/RAM Hot-plug Openstack

## Background

- Currently we can change the flavor and rebuilt the instance to add CPUs. It is not sopport adding CPUs to a running instance without rebuilt it to effect. We shoud have the ability add CPUs immediately without instance's state is changed

- vCPU, RAM hotplug feature support remove downtime when we need to change CPUs or RAM of an instance. More than it's support increase quality of service we provide

CPU hotplug APIs Based on KVM:

1. Add extra key to flavor to define the instance's current CPU

- Create VM with max VCPU in flavor

```
<vcpu current="2">4</vcpu>
```

2. Set metadata of the image if image has installed qemu-guest-agent.

```
nova image-meta {image_id} set hw_qemu_agent=yes
```

3. Provide the API of setting the new CPU of the instance

Ex: POST v2/{tenant_id}/servers/{server_id}/action

```
{
    "cpu_hotplug": {
        "vcpu": 3
    }
}
```
