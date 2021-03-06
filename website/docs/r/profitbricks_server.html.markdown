---
layout: "profitbricks"
page_title: "ProfitBricks: profitbricks_server"
sidebar_current: "docs-profitbricks-resource-server"
description: |-
  Creates and manages ProfitBricks Server objects.
---

# profitbricks_server

Manages a Server on ProfitBricks.

## Example Usage

This resource will create an operational server. After this section completes, the provisioner can be called.

```hcl
resource "profitbricks_server" "example" {
  name              = "server"
  datacenter_id     = "${profitbricks_datacenter.example.id}"
  cores             = 1
  ram               = 1024
  availability_zone = "ZONE_1"
  cpu_family        = "AMD_OPTERON"
  image_password    = "test1234"
  ssh_key_path      = "${var.private_key_path}"
  boot_image        = "${var.ubuntu}"

  volume {
    name           = "new"
    size           = 5
    disk_type      = "SSD"
  }

  nic {
    lan             = "${profitbricks_lan.example.id}"
    dhcp            = true
    ip              = "${profitbricks_ipblock.example.ips[0]}"
    firewall_active = true
  }
}
```

##Argument reference

- `name` - (Required)[string] The name of the server.
- `datacenter_id` - (Required)[string] The ID of a Virtual Data Center.
- `cores` - (Required)[integer] Number of server CPU cores.
- `ram` - (Required)[integer] The amount of memory for the server in MB.
- `availability_zone` - (Optional)[string] The availability zone in which the server should exist.
- `licence_type` - (Optional)[string] Sets the OS type of the server.
- `cpu_family` - (Optional)[string] Sets the CPU type. "AMD_OPTERON" or "INTEL_XEON". Defaults to "AMD_OPTERON".
- `volume` - (Required) See the Volume section.
- `nic` - (Required) See the NIC section.
- `boot_volume` - (Computed) The associated boot volume.
- `boot_cdrom` - (Computed) The associated boot drive, if any.
- `boot_image` - [string] The image or snapshot UUID / name. May also be an image alias. It is required if `licence_type` is not provided.
- `primary_nic` - (Computed) The associated NIC.
- `primary_ip` - (Computed) The associated IP address.
- `image_password` - (Computed) The associated IP address.
- `ssh_key_path` - (Required)[list] List of paths to files containing a public SSH key that will be injected into ProfitBricks provided Linux images. Required for ProfitBricks Linux images. Required if `image_password` is not provided.
- `image_password` - [string] Required if `sshkey_path` is not provided.

## Import

Resource Server can be imported using the `resource id`, e.g.

```shell
terraform import profitbricks_server.myserver {datacenter uuid}/{server uuid}/{primary_nic uuid}
# or
terraform import profitbricks_server.myserver {datacenter uuid}/{server uuid}/{primary_nic uuid}/{firewall uuid}
```

## Notes

Please note that for any secondary volume, you need to set the **licence_type** property to **UNKNOWN**
