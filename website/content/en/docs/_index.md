---
title: "Documentation"
linkTitle: "Documentation"
menu:
  main:
    weight: 20
---

AAD Pod Identity enables Kubernetes applications to access cloud resources securely with [Azure Active Directory](https://azure.microsoft.com/en-us/services/active-directory/).

Using Kubernetes primitives, administrators configure identities and bindings to match pods. Then without any code modifications, your containerized applications can leverage any resource in the cloud that depends on AAD as an identity provider.

## v1.7.0 Breaking Change

With [Azure/aad-pod-identity#842](https://github.com/Azure/aad-pod-identity/pull/842), aad-pod-identity no longer works on clusters with kubenet as the network plugin. For more details, please see [Deploy AAD Pod Identity in a Cluster with Kubenet](configure/aad_pod_identity_on_kubenet/).

If you still wish to install aad-pod-identity on a kubenet-enabled cluster, set the helm chart value `nmi.allowNetworkPluginKubenet` to `true` in the helm command:

```bash
helm (install|upgrade) ... --set nmi.allowNetworkPluginKubenet=true ...
```

## v1.6.0 Breaking Change

With [Azure/aad-pod-identity#398](https://github.com/Azure/aad-pod-identity/pull/398), the [client-go](https://github.com/kubernetes/client-go) library is upgraded to v0.17.2, where CRD [fields are now case sensitive](https://github.com/kubernetes/kubernetes/issues/64612). If you are upgrading MIC and NMI from v1.x.x to v1.6.0, MIC v1.6.0+ will upgrade the fields of existing `AzureIdentity` and `AzureIdentityBinding` on startup to the new format to ensure backward compatibility. A configmap called `aad-pod-identity-config` is created to record and confirm the successful type upgrade.

However, for future `AzureIdentity` and `AzureIdentityBinding` created using v1.6.0+, the following fields need to be changed:

### `AzureIdentity`

| < 1.6.0          | >= 1.6.0         |
| ---------------- | ---------------- |
| `ClientID`       | `clientID`       |
| `ClientPassword` | `clientPassword` |
| `ResourceID`     | `resourceID`     |
| `TenantID`       | `tenantID`       |

### `AzureIdentityBinding`

| < 1.6.0         | >= 1.6.0        |
| --------------- | --------------- |
| `AzureIdentity` | `azureIdentity` |
| `Selector`      | `selector`      |

### `AzurePodIdentityException`

| < 1.6.0     | >= 1.6.0    |
| ----------- | ----------- |
| `PodLabels` | `podLabels` |

## Ready to get started?

To get started, see the [Getting Started](./getting-started/) page, or you can visit the [GitHub repo](https://github.com/Azure/aad-pod-identity).
