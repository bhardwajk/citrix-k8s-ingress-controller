# Release notes

The Citrix ingress controller release notes describe the new features, enhancements to existing features, fixed issues, and known issues available in the release. The latest version of Citrix ingress controller is available in the [Quay.io](https://quay.io/repository/citrix/citrix-k8s-ingress-controller?tab=info) repository.

The release notes include one or more of the following sections:

-  [**What's new**](#whats-new): The new features and enhancements available in the current release.
-  [**Fixed issues**](#fixed-issues): The issues that are fixed in the current release.
-  [**Known issues**](#known-issues): The issues that exist in the current release and their workarounds, wherever applicable.

## Version 1.2.0

---

### What's New

#### Expose services of type LoadBalancer

You can create a service of type LoadBalancer and expose it externally using the ingress Citrix ADC. You can manually assign an IP address to the service using the `service.citrix.com/frontend-ip` annotation. Else, you can also automatically assign IP address to service using the IPAM controller provided by Citrix. The Citrix ingress controller configures the assigned IP address as virtual IP (VIP) in the ingress Citrix ADC. And, the service is exposed using the IP address. For more information, see [Expose services of type LoadBalancer](network/type_loadbalancer.md).

#### RedHat OpenShift router sharding support

OpenShift router sharding allows distributing a set of routes among multiple OpenShift routers. By default, an OpenShift router selects all routes from all namespaces. In router sharding, labels are added to routes or namespaces and label selectors to routers for filtering routes. Each router shard selects only routes with specific labels that match its label selection parameters.

Citrix ADC supports OpenShift router sharding when you deploy it as an OpenShift router. For more information, see [Deploy the Citrix ingress controller with OpenShift router sharding support](deploy/deploy-openshift-sharding.md).

#### Establish network connectivity between Kubernetes nodes and Ingress Citrix ADC using Citrix node controller

In Kubernetes environments, when you expose the services for external access through the Ingress device, to route the traffic into the cluster, you need to appropriately configure the network between the Kubernetes nodes and the Ingress device. Configuring the network is challenging as the pods use private IP addresses based on the CNI framework. Without proper network configuration, the Ingress device cannot access these private IP addresses. Also, manually configuring the network to ensure such reachability is cumbersome in Kubernetes environments.

Citrix provides a microservice called as **Citrix k8s node controller** that you can use to create the network between the cluster and the Ingress device. For more information, see [Citrix node controller](https://github.com/citrix/citrix-k8s-node-controller) and [Establish network between Kubernetes nodes and Ingress Citrix ADC using Citrix node controller](network/node-controller.md).


#### Ability to match the ingress path

The Citrix ingress controller now provides an annotation `ingress.citrix.com/path-match-method` that you can use to define the Citrix ingress controller to consider the path string in the ingress path has prefix expression or as an exact match. For more information, see [Annotations](configure/annotations.md).

#### Ability to customize the prefix for Citrix ADC entities

By default, the Citrix ingress controller adds "**k8s**" as prefix to the Citrix ADC entities such as, content switching (CS) virtual server, load balancing (LB) virtual server and so on. You can now customize the prefix using the `NS_APPS_NAME_PREFIX` environment variable in the Citrix ingress controller deployment YAML file. You can use alphanumeric characters for the prefix and the prefix length should not exceed 8 characters.

### Fixed issues

-  Preconfigured certificates with "**.**" in the certificate name is not supported. For example, hotdrink.cert.

    [[NSNET-10130](https://issues.citrite.net/browse/NSNET-10130)]

-  Citrix ingress controller fails to configure Citrix ADC if it is being deployed in standalone mode after rebooting Citrix ADC VPX.

    [[NSNET-10239]](https://issues.citrite.net/browse/NSNET-10239)

### Known issues

**Red Hat OpenShift support:**

-  [Automatic route configuration](network/staticrouting.md#automatically-configure-route-on-the-citrix-adc-instance) using the Citrix Ingress Controller (`feature-node-watch`) is not supported in OpenShift.

    [[#NSNET-8506]](https://issues.citrite.net/browse/NSNET-8506)

-  When you frequently modify the OpenShift route configuration, the Citrix ingress controller might crash with the following SSL exception: `SSL: DECRYPTION_FAILED_OR_BAD_RECORD_MAC`.

    [[#NSNET-10027]](https://issues.citrite.net/browse/NSNET-10027)

-  After modifying the OpenShift route configuration, applying those changes using the `oc apply` command does not work.

    [[NSNET-10264]](https://issues.citrite.net/browse/NSNET-10264)

    **Workaround:** Delete the existing OpenShift route and recreate the route.

**Rewrite policy CRD:**

-  When you apply the rewrite policy CRD deployment file on the Kubernetes cluster, Citrix ingress controller requires 12 seconds to process the CRD deployment file.

    [[NSNET-8315]](https://issues.citrite.net/browse/NSNET-8315)

---

## Previous releases

### Version 1.1.3

#### What's New

**Red Hat OpenShift support**

The Citrix ingress controller can now be deployed as an OpenShift [router plug-in](https://docs.openshift.com/container-platform/3.9/architecture/networking/assembly_available_router_plugins.html). Also, Citrix ADC CPX can be deployed as a router within the OpenShift cluster. For more information, see [Deploy the Citrix ingress controller as an OpenShift router plug-in](deploy/deploy-cic-openshift.md).

**Expose services using NodePort**

You can create the service of type [NodePort](https://kubernetes.io/docs/concepts/services-networking/service/#nodeport) and make the services accessible from outside of the Kubernetes cluster. The Citrix ADC (VPX or MPX) instance outside the Kubernetes cluster load balances the Ingress traffic to the nodes that contain the pods running the services. For more information, see [Expose services using NodePort](network/nodeport.md).

**Using Citrix ADC with admin partitions as ingress device**

The Citrix ingress controller can now be deployed to automatically configure Citrix ADC with admin partitions based on the Ingress resource configuration. For more information, see [Deploy the Citrix ingress controller for Citrix ADC with admin partitions](deploy/deploy-cic-adc-admin-partition.md).

**Rancher managed Kubernetes cluster support**

The Citrix ingress controller can now be deployed on a Rancher managed Kubernetes cluster. For more information, see [Deploy the Citrix ingress controller on a Rancher managed Kubernetes cluster](deploy/deploy-cic-rancher.md).

#### Known issues

**Red Hat OpenShift support:**

-  [Automatic route configuration](network/staticrouting.md#automatically-configure-route-on-the-citrix-adc-instance) using the Citrix Ingress Controller (`feature-node-watch`) is not supported in OpenShift.

    [[#NSNET-8506]](https://issues.citrite.net/browse/NSNET-8506)

-  The router sharding feature in OpenShift is not supported.

    [[#NSNET-8658]](https://issues.citrite.net/browse/NSNET-8658)

-  When you frequently modify the OpenShift route configuration, the Citrix ingress controller might crash with the following SSL exception: `SSL: DECRYPTION_FAILED_OR_BAD_RECORD_MAC`.

    [[#NSNET-10027]](https://issues.citrite.net/browse/NSNET-10027)

-  After modifying the OpenShift route configuration, applying those changes using the `oc apply` command does not work.

    [[NSNET-10264]](https://issues.citrite.net/browse/NSNET-10264)

    **Workaround:** Delete the existing OpenShift route and recreate the route.

**Rewrite policy CRD:**

-  When you apply the rewrite policy CRD deployment file on the Kubernetes cluster, Citrix ingress controller requires 12 seconds to process the CRD deployment file.

    [[NSNET-8315]](https://issues.citrite.net/browse/NSNET-8315)
  
**Other issues:**

-  Citrix ingress controller fails to configure Citrix ADC if it is being deployed in standalone mode after rebooting Citrix ADC VPX.

    [[NSNET-10239]](https://issues.citrite.net/browse/NSNET-10239)

     **Workaround:** Delete the Citrix ingress controller and redeploy it again.

**Points to note:**

If you are using the UDP related ingress, you must perform the following steps while upgrading the Citrix ingress controller:

1.  Remove the UDP related Ingress configuration.
1.  Upgrade the Citrix ingress controller.
1.  Update each UDP ingress YAML file as mentioned in [UDP-based Ingress](https://developer-docs.citrix.com/projects/citrix-k8s-ingress-controller/en/latest/how-to/tcp-udp-ingress/).
1.  Reapply the UDP related Ingress configuration.
