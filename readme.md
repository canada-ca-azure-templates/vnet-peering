
# Virtual Network Peering

## Introduction

This template is used to [peer vnets](<https://docs.microsoft.com/en-us/azure/templates/microsoft.network/2018-11-01/virtualnetworks/virtualnetworkpeerings>) so that they can communicate with one another.

## Security Controls

* Unknown

## Dependancies

The deployment assumes the following items are already deployed:

* [Resource Group](<https://github.com/canada-ca-azure-templates/resourcegroups>)
* [Virtal Network](<https://github.com/canada-ca-azure-templates/vnet-subnet>)

## Parameter format

```JSON
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "containerSasToken": {
            "value": "[SasToken]"
        },
        "peerArray": {
            "value": [
                {
                    "resourceGroup": "rgTestCentral",
                    "vnet": "vnet-core",
                    "peer": [
                        {
                            "vnetName": "vnet-crm-dev",
                            "rgName": "rgTestCentral",
                            "allowVirtualNetworkAccess": true,
                            "allowForwardedTraffic": false,
                            "allowGatewayTransit": true,
                            "useRemoteGateways": false
                        },
                        {
                            "vnetName": "vnet-management",
                            "rgName": "rgTestCentral",
                            "allowVirtualNetworkAccess": true,
                            "allowForwardedTraffic": false,
                            "allowGatewayTransit": true,
                            "useRemoteGateways": false
                        }
                    ]
                }
            ]
        }
    }
}
```

## Parameter Values

### Main Template

| Name              | Type   | Required | Value                                            |
| ----------------- | ------ | -------- | ------------------------------------------------ |
| containerSasToken | string | No       | A SaS token for the private blob storage         |
| peerArray         | Array  | Yes      | Array of [PeerArray Object](#peerarray-object) |

### PeerArray Object

| Name          | Type   | Required | Value                                 |
| ------------- | ------ | -------- | ------------------------------------- |
| resourceGroup | string | Yes      | The resource group for the deployment |
| vnet          | string | Yes      | The name of the primary vnet to peer  |
| peer          | Array  | Yes      | [Peer Object](#peer-object)         |

### Peer Object

| Name                      | Type   | Required | Value                                                                                                                                                                                                                                                                                                                                   |
| ------------------------- | ------ | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| vnetName                  | string | Yes      | The name of the secondary vnet to pair.                                                                                                                                                                                                                                                                                                 |
| rgName                    | string | Yes      | The resource group for the vnet.                                                                                                                                                                                                                                                                                                        |
| subscriptionId            | string | No       | Resource ID of the vnet you want to peer with in a different subscription. Eg: /subscriptions/44c891bd-f610-4e4c-a6b9-8f0e586405a1/resourceGroups/Test-VNET-Peering-RG/providers/Microsoft.Network/virtualNetworks/Test-Sub2-VNET                                                                                                       |
| allowVirtualNetworkAccess | bool   | Yes      | Whether the VMs in the linked virtual network space would be able to access all the VMs in local Virtual network space.                                                                                                                                                                                                                 |
| allowForwardedTraffic     | bool   | Yes      | Whether the forwarded traffic from the VMs in the remote virtual network will be allowed/disallowed.                                                                                                                                                                                                                                    |
| allowGatewayTransit       | bool   | Yes      | If gateway links can be used in remote virtual networking to link to this virtual network.                                                                                                                                                                                                                                              |
| useRemoteGateways         | bool   | Yes      | If remote gateways can be used on this virtual network. If the flag is set to true, and allowGatewayTransit on remote peering is also true, virtual network will use gateways of remote virtual network for transit. Only one peering can have this flag set to true. This flag cannot be set if virtual network already has a gateway. |

## History

| Date     | Change                                                                                                                     |
| -------- | -------------------------------------------------------------------------------------------------------------------------- |
| 20181120 | Adding helpers folder and getParameters.json template to provide method to read parameter files from parent link template. |
| 20181214 | Implementing new template name as template.json                                                                            |
| 20190205 | Cleanup template folder                                                                                                    |
| 20190430 | Cleanup documentation                                                                                                      |
| 20190506 | Fix cross subscription peering issue and update readme                                                                     |
| 20190516 | [20190516](https://github.com/canada-ca-azure-templates/vnet-peering/tree/20190516) | Created test validation.                                   |