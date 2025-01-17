# Troubleshooting

The following table describes some of the common issues and workarounds.

|**Problem**|**Log**|**Workaround**|
|------|-----|-----|
|Citrix ADC instance is not reachable|**2019-01-10 05:05:27,250 - ERROR - [nitrointerface.py:login_logout:94] (MainThread) Exception: HTTPConnectionPool(host='192.106.76.200', port=80): Max retries exceeded with url: /nitro/v1/config/login (Caused by NewConnectionError('<urllib3.connection.HTTPConnection object at 0x7f4d45bd63d0>: Failed to establish a new connection: [Errno 113] No route to host',))**|Ensure that Citrix ADC is up and running, and you can ping the NSIP address.|
|Wrong user name password|**2019-01-10 05:03:05,958  - ERROR - [nitrointerface.py:login_logout:90] (MainThread) Nitro Exception::login_logout::errorcode=354,message=Invalid username or password**| |
|SNIP is not enabled with management access|**2019-01-10 05:43:03,418  - ERROR - [nitrointerface.py:login_logout:94] (MainThread) Exception: HTTPConnectionPool(host='192.106.76.242', port=80): Max retries exceeded with url: /nitro/v1/config/login (Caused by NewConnectionError('<urllib3.connection.HTTPConnection object at 0x7f302a8cfad0>: Failed to establish a new connection: [Errno 110] Connection timed out',))**|Ensure that you have enabled the management access in Citrix ADC (for Citrix ADC VPX high availability) and set the IP address, **NSIP**, with management access enbled.|
|Error while parsing annotations|**2019-01-10 05:16:10,611 - ERROR - [kubernetes.py:set_annotations_to_csapp:1040] (MainThread) set_annotations_to_csapp: Error message=No JSON object could be decodedInvalid Annotation $service_weights please fix and apply ${"frontend":, "catalog":95}**| |
|Wrong port for NITRO access|**2019-01-10 05:18:53,964 - ERROR - [nitrointerface.py:login_logout:94] (MainThread) Exception: HTTPConnectionPool(host='192.106.76.242', port=34438): Max retries exceeded with url: /nitro/v1/config/login (Caused by NewConnectionError('<urllib3.connection.HTTPConnection object at 0x7fc592cb8b10>: Failed to establish a new connection: [Errno 111] Connection refused',))**|Verify if the correct port is specified for NITRO access. By default, Citrix ingress controller uses port **80** for communcation.|
| Ingress class is wrong|**2019-01-10 05:27:27,149  - INFO - [kubernetes.py:get_all_ingresses:1329] (MainThread) Unsupported Ingress class for ingress object web-ingress.default**|Verify that the ingress file belongs to the ingress class that Citrix ingress controller monitors. See the following log for information about the ingress classes listened by Citrix ingress controller: <br> Log: 2019-01-10 05:27:27,120 - DEBUG - [kubernetes.py:__init__:63] (MainThread) Ingress classes allowed: <br> 2019-01-10 05:27:27,120 - DEBUG - [kubernetes.py:__init__:64] (MainThread) ['vpxclass']|
|Kubernetes API is not reachable|**2019-01-10 05:32:09,729  - ERROR - [kubernetes.py:_get:222] (Thread-1) Error while calling /services:HTTPSConnectionPool(host='192.106.76.237', port=6443): Max retries exceeded with url: /api/v1/services (Caused by NewConnectionError('<urllib3.connection.VerifiedHTTPSConnection object at 0x7fb3013e7dd0>: Failed to establish a new connection: [Errno 111] Connection refused',))**| Check if the kubernetes_url is correct. Use the command, `kubectl cluster-info` to get the URL information. Ensure that the Kubernetes master is running at `https://kubernetes_master_address:6443` and also the Kubernetes API server pod is up and running. |
| Incorrect service port specified in the YAML file| |Provide the correct port details in the ingress YAML file and reapply to solve the issue. |
|Load balancing virtual server and service group are created but they are down| |Check for the service name and port used in the YAML file. For Citrix ADC VPX, ensure that `--feature-node-watch` is set to `true`, when bringing up the Citrix ingress controller.|
|CS virtual server is not getting created for Citrix ADC VPX.| |Use the annotation, `ingress.citrix.com/frontend-ip`, in the ingress YAML file for Citrix ADC VPX.|
|Incorrect secret provided in TLS section in the ingress YAML file|**2019-01-10 09:30:50,673 - INFO - [kubernetes.py:_get:231] (MainThread) Resource not found: /secrets/default-secret12345 namespace default** <br> **2019-01-10 09:30:50,673 - INFO - [kubernetes.py:get_secret:1712] (MainThread) Failed to get secret for the app default-secret12345.default**|Correct the values in the YAML file and reapply to solve the issue. |
|The `feature-node-watch` argument is specified but static routes are not added in the Citrix ADC VPX|**ERROR - [nitrointerface.py:add_ns_route:4495] (MainThread) Nitro Exception::add_ns_route::errorcode=604,message=The gateway is not directly reachable**|This error occurs when `feature-node-watch` is enabled when Citrix ADC VPX and Kubernetes cluster is not in the same network. The`- --feature-node-watch` argument needs to be removed from the Citrix ingress controller YAML file. Static routes do not work when Citrix ADC VPX and Kubernetes cluster are in different network.You need to use Citrix node controller to create tunnels between Citrix ADC VPX and cluster nodes.|
|CRD status not updated|**ERROR - [crdinfrautils.py:update_crd_status:42] (MainThread) Exception during CRD status update for negrwaddmuloccmod: 403 Client Error: Forbidden for url: https://10.96.0.1:443/apis/citrix.com/v1/namespaces/default/rewritepolicies/negrwaddmuloccmod/status**| Verify that permission to push CRD status is provided in the RBAC. The permission should be similar to the following: <BR> - apiGroups: ["citrix.com"] resources: ["rewritepolicies/status", "canarycrds/status", "authpolicies/status", "ratelimits/status", "listeners/status", "httproutes/status", "wafs/status"] |
|Citrix ingress controller event not updated| **ERROR - [clienthelper.py:post:94] (MainThread) Reuqest /events to api server is forbidden** | Verify that the permission to update the Citrix ingress controller pod events is provided in the RBAC. <BR> - apiGroups: [""] resources: ["events"] verbs: ["create"] |
|Rewrite-responder policy not added| **ERROR - [config_dispatcher.py:__dispatch_config_pack:324] (Dispatcher) Status: 104, ErrorCode: 3081, Reason: Nitro Exception: Expression syntax error [D(10, 20).^RE_SELECT(, Offset 15] <BR> ERROR - [config_dispatcher.py:__dispatch_config_pack:324] (Dispatcher) Status: 104, ErrorCode: 3098, Reason: Nitro Exception: Invalid expression data type [ent.ip.src^, Offset 13]** | Such errors are usually due to incorrect expressions in rewrite-responder CRDs. Fix the expression and reapply the CRD. |
| Application of a CRD failed. The Citrix ingress controller converts a CRD in to a set of configurations to configure Citrix ADC to the desired state with respect to the specified CRD. If the configuration fails, then the CRD instance may not get applied on the Citrix ADC.| **2020-07-13 08:49:07,620  - ERROR - [config_dispatcher.py:__dispatch_config_pack:256] (Dispatcher) Failed to execute config ADD_sslprofile_k8s_crd_k8service_kuard-service_default_80_tcp_backend_{name:k8s_crd_k8service_kuard-service_default_80_tcp_backend sslprofiletype:BackEnd tls12:enabled } from ConfigPack 'default.k8service.kuard-service.add_spec' <BR>2020-07-13 08:49:07,620  - ERROR - [config_dispatcher.py:__dispatch_config_pack:257] (Dispatcher) Status: 104, ErrorCode: 1074, Reason: Nitro Exception: Invalid value [sslProfileType, value differs from existing entity and it cant be updated.]<BR>2020-07-13 08:49:07,620  - INFO - [config_dispatcher.py:__dispatch_config_pack:263] (Dispatcher) Processing of ConfigPack 'default.k8service.kuard-service.add_spec' failed** | Log shows that the Nitro command has failed. The same log will appear in Citrix ADC as well. Check the Citrix ADC `ns.log` and search for the error string using the `grep` command to figure out the Citrix ADC command which failed during the application of CRD. Try to delete the CRD and add it again. If you see the issue again, report it on the cloud native slack channel.|


## Troubleshooting - Prometheus and Grafana Integration

|**Problem**|**Description**|**Workaround**|
|------|-----|-----|
|Grafana dashboard has no plots|If the graphs on the Grafana dashboards do not have any values plotted, then Grafana is unable to obtain statistics from its datasource.| Check if the Prometheus datasource is saved and working properly. On saving the datasource after providing the Name and IP, a "Data source is working" message appears in green indicating the datasource is reachable and detected. <br>If the dashboard is created using `sample_grafana_dashboard.json`, ensure that the name given to the Prometheus datasource begins with the word "prometheus" in lowercase. <br>Check the Targets page of Prometheus to see if the required target exporter is in `DOWN` state.|
| DOWN: Context deadline exceeded| If the message appears against any of the exporter targets of Prometheus, then Prometheus is either unable to connect to the exporter or unable to fetch all the metrics within the given `scrape_timeout`.|If you are using Prometheus Operator, `scrape_timeout` is adjusted automatically and the error means that the exporter itself is not reachable. <br>If a standalone Prometheus container or pod is used, try increasing the `scrape_interval` and `scrape_timeout` values in the `/etc/prometheus/prometheus.cfg` file to increase the time interval for collecting the metrics.|

## Troubleshooting - OpenShift feature node watch

**Problem 1**:
While using OpenShift-ovn CNI `feature-node-watch` is not adding correct routes.

**Description**: Citrix ingress controller looks for Node annotations for fetching the necessary details to add the static routes.

**Workaround**:

1. Make sure that following RBAC permission is provided to Citrix ingress controller along with `route.openshift.io` for Citrix ingress controller to run in the OpenShift environment with OVN CNI.

        - apiGroups: ["config.openshift.io"]
          resources: ["networks"]
          verbs: ["get", "list"]

2. Citrix ingress controller looks for the following two annotations added by OVN, make sure that it exists on the 
cluster nodes.

        "k8s.ovn.org/node-subnets": {\"default\":\"10.128.0.0/23\"}",
        "k8s.ovn.org/node-primary-ifaddr": "{\"ipv4\":\"x.x.x.x/24\"}"

3. If the annotation does not exist `feature-node-watch`  might not work for OVN CNI. In that case, you must manually configure the static routes on Citrix ADC VPX.

**Problem 2**:
While using OpenShift-sdn CNI feature-node-watch is not adding correct routes

**Description**: Citrix ingress controller looks for the Hostsubnet CRD for fetching the necessary details to add the static routes.

**Workaround**:

1. Make sure that following RBAC permission is provided to Citrix ingress controller along with  `route.openshift.io` for Citrix ingress controller to run in the OpenShift environment with SDN CNI.

        - apiGroups: ["network.openshift.io"]
          resources: ["hostsubnets"]
          verbs: ["get", "list", "watch"]
        - apiGroups: ["config.openshift.io"]
          resources: ["networks"]
          verbs: ["get", "list"]
2. Citrix ingress controller looks for the following CRD and specification.

            oc get hostsubnets.network.openshift.io <cluster node-name> -ojson

            { "apiVersion": "network.openshift.io/v1",
                "host": <cluster node-name, 
                "hostIP": "x.x.x.x",
                "kind": "HostSubnet",
                "metadata": {
            "annotations": {
                            ...
            },
                "subnet": "10.129.0.0/23"
            }
3. If the CRD does not exist with the expected specification, `feature-node-watch` might not work for OpenShfit-SDN CNI. In that case, you must manually configure the static routes on Citrix ADC VPX.
