**NGINX Ingress controller version** (exec into the pod and run nginx-ingress-controller --version.): 1.0.0-dev

**Kubernetes version** (use `kubectl version`): observed in 1.20 and 1.21

**Environment**: development

- **Cloud provider or hardware configuration**: Observed in GKE, but replicated in local kind cluster running on a macbook
- **OS** (e.g. from /etc/os-release): 
- **Kernel** (e.g. `uname -a`):
- **Install tools**: installed on a local kind cluster following the developer getting started guide here https://github.com/kubernetes/ingress-nginx/blob/main/docs/developer-guide/getting-started.md
- **Basic cluster related info**:
  - `kubectl version` - Server Version: v1.21.1
  - `kubectl get nodes -o wide` - 
  
        ingress-nginx-dev-control-plane   Ready    control-plane,master   2d13h   v1.21.1   172.19.0.2    <none>        Ubuntu 21.04   5.10.76-linuxkit   containerd://1.5.2

- **How was the ingress-nginx-controller installed**: The controller was installed on a local kind cluster following the developer getting started guide here https://github.com/kubernetes/ingress-nginx/blob/main/docs/developer-guide/getting-started.md. Specifically the command to create the environment and install was simply `make dev-env`.

- **Current State of the controller**:
  - `kubectl describe ingressclasses`

        Name:         nginx
        Labels: app.kubernetes.io/component=controller
                app.kubernetes.io/instance=ingress-nginx
                app.kubernetes.io/managed-by=Helm
                app.kubernetes.io/name=ingress-nginx
                app.kubernetes.io/part-of=ingress-nginx
                app.kubernetes.io/version=1.1.1
                helm.sh/chart=ingress-nginx-4.0.17
        Annotations:  <none>
        Controller:   k8s.io/ingress-nginx
        Events:       <none>

  - `kubectl -n <ingresscontrollernamespace> get all -A -o wide`

        NAMESPACE            NAME                                                          READY   STATUS      RESTARTS   AGE     IP            NODE                              NOMINATED NODE   READINESS GATES
        ingress-nginx        pod/ingress-nginx-admission-create-pnmqv                      0/1     Completed   0          2d13h   10.244.0.5    ingress-nginx-dev-control-plane   <none>           <none>
        ingress-nginx        pod/ingress-nginx-admission-patch-mwf9b                       0/1     Completed   1          2d13h   10.244.0.6    ingress-nginx-dev-control-plane   <none>           <none>
        ingress-nginx        pod/ingress-nginx-controller-75bbff5895-c9s46                 1/1     Running     0          2d13h   10.244.0.7    ingress-nginx-dev-control-plane   <none>           <none>
        kube-system          pod/coredns-558bd4d5db-m4zng                                  1/1     Running     0          2d13h   10.244.0.4    ingress-nginx-dev-control-plane   <none>           <none>
        kube-system          pod/coredns-558bd4d5db-rzrvw                                  1/1     Running     0          2d13h   10.244.0.3    ingress-nginx-dev-control-plane   <none>           <none>
        kube-system          pod/etcd-ingress-nginx-dev-control-plane                      1/1     Running     0          2d13h   172.19.0.2    ingress-nginx-dev-control-plane   <none>           <none>
        kube-system          pod/kindnet-5zw2z                                             1/1     Running     0          2d13h   172.19.0.2    ingress-nginx-dev-control-plane   <none>           <none>
        kube-system          pod/kube-apiserver-ingress-nginx-dev-control-plane            1/1     Running     0          2d13h   172.19.0.2    ingress-nginx-dev-control-plane   <none>           <none>
        kube-system          pod/kube-controller-manager-ingress-nginx-dev-control-plane   1/1     Running     1          2d13h   172.19.0.2    ingress-nginx-dev-control-plane   <none>           <none>
        kube-system          pod/kube-proxy-frm62                                          1/1     Running     0          2d13h   172.19.0.2    ingress-nginx-dev-control-plane   <none>           <none>
        kube-system          pod/kube-scheduler-ingress-nginx-dev-control-plane            1/1     Running     1          2d13h   172.19.0.2    ingress-nginx-dev-control-plane   <none>           <none>
        local-path-storage   pod/local-path-provisioner-547f784dff-4wjhw                   1/1     Running     2          2d13h   10.244.0.2    ingress-nginx-dev-control-plane   <none>           <none>
        testing              pod/pod1-758f994ccb-s88sb                                     1/1     Running     0          2d12h   10.244.0.16   ingress-nginx-dev-control-plane   <none>           <none>
        testing              pod/pod2-6588cb6c5c-ckwbk                                     1/1     Running     0          2d12h   10.244.0.17   ingress-nginx-dev-control-plane   <none>           <none>

        NAMESPACE       NAME                                         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE     SELECTOR
        default         service/kubernetes                           ClusterIP   10.96.0.1       <none>        443/TCP                      2d13h   <none>
        ingress-nginx   service/ingress-nginx-controller             NodePort    10.96.164.106   <none>        80:32578/TCP,443:32461/TCP   2d13h   app.kubernetes.io/component=controller,app.kubernetes.io/instance=ingress-nginx,app.kubernetes.io/name=ingress-nginx
        ingress-nginx   service/ingress-nginx-controller-admission   ClusterIP   10.96.166.101   <none>        443/TCP                      2d13h   app.kubernetes.io/component=controller,app.kubernetes.io/instance=ingress-nginx,app.kubernetes.io/name=ingress-nginx
        kube-system     service/kube-dns                             ClusterIP   10.96.0.10      <none>        53/UDP,53/TCP,9153/TCP       2d13h   k8s-app=kube-dns
        testing         service/service1                             ClusterIP   10.96.117.197   <none>        80/TCP                       64m     app=pod2

        NAMESPACE     NAME                        DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR            AGE     CONTAINERS    IMAGES                                          SELECTOR
        kube-system   daemonset.apps/kindnet      1         1         1       1            1           <none>                   2d13h   kindnet-cni   docker.io/kindest/kindnetd:v20210326-1e038dc5   app=kindnet
        kube-system   daemonset.apps/kube-proxy   1         1         1       1            1           kubernetes.io/os=linux   2d13h   kube-proxy    k8s.gcr.io/kube-proxy:v1.21.1                   k8s-app=kube-proxy

        NAMESPACE            NAME                                       READY   UP-TO-DATE   AVAILABLE   AGE     CONTAINERS               IMAGES                                                  SELECTOR
        ingress-nginx        deployment.apps/ingress-nginx-controller   1/1     1            1           2d13h   controller               gcr.io/k8s-staging-ingress-nginx/controller:1.0.0-dev   app.kubernetes.io/component=controller,app.kubernetes.io/instance=ingress-nginx,app.kubernetes.io/name=ingress-nginx
        kube-system          deployment.apps/coredns                    2/2     2            2           2d13h   coredns                  k8s.gcr.io/coredns/coredns:v1.8.0                       k8s-app=kube-dns
        local-path-storage   deployment.apps/local-path-provisioner     1/1     1            1           2d13h   local-path-provisioner   docker.io/rancher/local-path-provisioner:v0.0.14        app=local-path-provisioner
        testing              deployment.apps/pod1                       1/1     1            1           2d13h   pod1                     nginx:latest                                            app=pod1
        testing              deployment.apps/pod2                       1/1     1            1           2d13h   pod2                     nginx:latest                                            app=pod2

        NAMESPACE            NAME                                                  DESIRED   CURRENT   READY   AGE     CONTAINERS               IMAGES                                                  SELECTOR
        ingress-nginx        replicaset.apps/ingress-nginx-controller-75bbff5895   1         1         1       2d13h   controller               gcr.io/k8s-staging-ingress-nginx/controller:1.0.0-dev   app.kubernetes.io/component=controller,app.kubernetes.io/instance=ingress-nginx,app.kubernetes.io/name=ingress-nginx,pod-template-hash=75bbff5895
        kube-system          replicaset.apps/coredns-558bd4d5db                    2         2         2       2d13h   coredns                  k8s.gcr.io/coredns/coredns:v1.8.0                       k8s-app=kube-dns,pod-template-hash=558bd4d5db
        local-path-storage   replicaset.apps/local-path-provisioner-547f784dff     1         1         1       2d13h   local-path-provisioner   docker.io/rancher/local-path-provisioner:v0.0.14        app=local-path-provisioner,pod-template-hash=547f784dff
        testing              replicaset.apps/pod1-5df95d959f                       0         0         0       2d13h   pod1                     nginx:latest                                            app=pod1,pod-template-hash=5df95d959f
        testing              replicaset.apps/pod1-758f994ccb                       1         1         1       2d12h   pod1                     nginx:latest                                            app=pod1,pod-template-hash=758f994ccb
        testing              replicaset.apps/pod1-78456dbb8c                       0         0         0       2d12h   pod1                     nginx:latest                                            app=pod1,pod-template-hash=78456dbb8c
        testing              replicaset.apps/pod1-8cdcdfbbc                        0         0         0       2d13h   pod1                     nginx:latest                                            app=pod1,pod-template-hash=8cdcdfbbc
        testing              replicaset.apps/pod1-99cdffc57                        0         0         0       2d13h   pod1                     nginx:latest                                            app=pod1,pod-template-hash=99cdffc57
        testing              replicaset.apps/pod2-566f6847c4                       0         0         0       2d13h   pod2                     nginx:latest                                            app=pod2,pod-template-hash=566f6847c4
        testing              replicaset.apps/pod2-6588cb6c5c                       1         1         1       2d12h   pod2                     nginx:latest                                            app=pod2,pod-template-hash=6588cb6c5c

        NAMESPACE       NAME                                       COMPLETIONS   DURATION   AGE     CONTAINERS   IMAGES                                                                                                                         SELECTOR
        ingress-nginx   job.batch/ingress-nginx-admission-create   1/1           4s         2d13h   create       k8s.gcr.io/ingress-nginx/kube-webhook-certgen:v1.1.1@sha256:64d8c73dca984af206adf9d6d7e46aa550362b1d7a01f3a0a91b20cc67868660   controller-uid=fbc02b30-75ca-4853-ae31-bccea84ae7d5
        ingress-nginx   job.batch/ingress-nginx-admission-patch    1/1           6s         2d13h   patch        k8s.gcr.io/ingress-nginx/kube-webhook-certgen:v1.1.1@sha256:64d8c73dca984af206adf9d6d7e46aa550362b1d7a01f3a0a91b20cc67868660   controller-uid=9ce02d2d-1e82-4dc7-981d-5d533c8edccf

  - `kubectl -n <ingresscontrollernamespace> describe po <ingresscontrollerpodname>`

        Name:         ingress-nginx-controller-75bbff5895-c9s46
        Namespace:    ingress-nginx
        Priority:     0
        Node:         ingress-nginx-dev-control-plane/172.19.0.2
        Start Time:   Fri, 04 Mar 2022 15:09:38 -0600
        Labels:       app.kubernetes.io/component=controller
                    app.kubernetes.io/instance=ingress-nginx
                    app.kubernetes.io/name=ingress-nginx
                    deploy-date=1646428176
                    pod-template-hash=75bbff5895
        Annotations:  <none>
        Status:       Running
        IP:           10.244.0.7
        IPs:
        IP:           10.244.0.7
        Controlled By:  ReplicaSet/ingress-nginx-controller-75bbff5895
        Containers:
        controller:
            Container ID:  containerd://83ca6e52988c074b75730c43900b4ae56bcaff1e115f8e59554ed24f791c7a69
            Image:         gcr.io/k8s-staging-ingress-nginx/controller:1.0.0-dev
            Image ID:      sha256:1ad864a1cec8b1c075bfc3ecbb874fd409708fbbd1d55beab09d22dc0aa7f4b3
            Ports:         80/TCP, 443/TCP, 8443/TCP
            Host Ports:    80/TCP, 443/TCP, 0/TCP
            Args:
            /nginx-ingress-controller
            --publish-service=$(POD_NAMESPACE)/ingress-nginx-controller
            --election-id=ingress-controller-leader
            --controller-class=k8s.io/ingress-nginx
            --ingress-class=nginx
            --configmap=$(POD_NAMESPACE)/ingress-nginx-controller
            --validating-webhook=:8443
            --validating-webhook-certificate=/usr/local/certificates/cert
            --validating-webhook-key=/usr/local/certificates/key
            State:          Running
            Started:      Fri, 04 Mar 2022 15:09:46 -0600
            Ready:          True
            Restart Count:  0
            Requests:
            cpu:      100m
            memory:   90Mi
            Liveness:   http-get http://:10254/healthz delay=10s timeout=1s period=10s #success=1 #failure=5
            Readiness:  http-get http://:10254/healthz delay=10s timeout=1s period=10s #success=1 #failure=3
            Environment:
            POD_NAME:       ingress-nginx-controller-75bbff5895-c9s46 (v1:metadata.name)
            POD_NAMESPACE:  ingress-nginx (v1:metadata.namespace)
            LD_PRELOAD:     /usr/local/lib/libmimalloc.so
            Mounts:
            /usr/local/certificates/ from webhook-cert (ro)
            /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-wcdbh (ro)
        Conditions:
        Type              Status
        Initialized       True
        Ready             True
        ContainersReady   True
        PodScheduled      True
        Volumes:
        webhook-cert:
            Type:        Secret (a volume populated by a Secret)
            SecretName:  ingress-nginx-admission
            Optional:    false
        kube-api-access-wcdbh:
            Type:                    Projected (a volume that contains injected data from multiple sources)
            TokenExpirationSeconds:  3607
            ConfigMapName:           kube-root-ca.crt
            ConfigMapOptional:       <nil>
            DownwardAPI:             true
        QoS Class:                   Burstable
        Node-Selectors:              kubernetes.io/os=linux
        Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                                    node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
        Events:                      <none>

  - `kubectl -n <ingresscontrollernamespace> describe svc <ingresscontrollerservicename>` - n/a

- **Current state of ingress object, if applicable**:
  - `kubectl -n <appnnamespace> get all,ing -o wide`

        NAME                        READY   STATUS    RESTARTS   AGE     IP            NODE                              NOMINATED NODE   READINESS GATES
        pod/pod1-758f994ccb-s88sb   1/1     Running   0          2d12h   10.244.0.16   ingress-nginx-dev-control-plane   <none>           <none>
        pod/pod2-6588cb6c5c-ckwbk   1/1     Running   0          2d12h   10.244.0.17   ingress-nginx-dev-control-plane   <none>           <none>

        NAME               TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE   SELECTOR
        service/service1   ClusterIP   10.96.117.197   <none>        80/TCP    69m   app=pod2

        NAME                   READY   UP-TO-DATE   AVAILABLE   AGE     CONTAINERS   IMAGES         SELECTOR
        deployment.apps/pod1   1/1     1            1           2d13h   pod1         nginx:latest   app=pod1
        deployment.apps/pod2   1/1     1            1           2d13h   pod2         nginx:latest   app=pod2

        NAME                              DESIRED   CURRENT   READY   AGE     CONTAINERS   IMAGES         SELECTOR
        replicaset.apps/pod1-5df95d959f   0         0         0       2d13h   pod1         nginx:latest   app=pod1,pod-template-hash=5df95d959f
        replicaset.apps/pod1-758f994ccb   1         1         1       2d12h   pod1         nginx:latest   app=pod1,pod-template-hash=758f994ccb
        replicaset.apps/pod1-78456dbb8c   0         0         0       2d13h   pod1         nginx:latest   app=pod1,pod-template-hash=78456dbb8c
        replicaset.apps/pod1-8cdcdfbbc    0         0         0       2d13h   pod1         nginx:latest   app=pod1,pod-template-hash=8cdcdfbbc
        replicaset.apps/pod1-99cdffc57    0         0         0       2d13h   pod1         nginx:latest   app=pod1,pod-template-hash=99cdffc57
        replicaset.apps/pod2-566f6847c4   0         0         0       2d13h   pod2         nginx:latest   app=pod2,pod-template-hash=566f6847c4
        replicaset.apps/pod2-6588cb6c5c   1         1         1       2d12h   pod2         nginx:latest   app=pod2,pod-template-hash=6588cb6c5c

        NAME                                 CLASS   HOSTS                      ADDRESS         PORTS   AGE
        ingress.networking.k8s.io/ingress1   nginx   testing.myfakedomain.com   10.96.164.106   80      74m

  - `kubectl -n <appnamespace> describe ing <ingressname>`

        Name:             ingress1
        Namespace:        testing
        Address:          10.96.164.106
        Default backend:  default-http-backend:80 (<error: endpoints "default-http-backend" not found>)
        Rules:
        Host                      Path  Backends
        ----                      ----  --------
        testing.myfakedomain.com
                                    /   service1:80 (10.244.0.17:80)
        Annotations:                testing: true
        Events:                     <none>

  - If applicable, then, your complete and exact curl/grpcurl command (redacted if required) and the reponse to the curl/grpcurl command with the -v flag

- **Others**:
  - Any other related information


**What happened**:

- Initially the Service `service` was created as type `ExternalName` pointing to Pod `pod1`. A curl to the fake domain will respond with text `i am pod1` indicating traffic is being forwarded to pod1 successfully. 
- The service is then changed in place to a type `ClusterIP` with a selector pointing to Pod `pod2`. A curl to the fake domain should respond with `i am pod2`, however the response is `i am pod1`.
- If the service selector is changed to point to a different pod (ie pod3), a curl will show that ingress-nginx is still forwarding to pod1. 
- If the service is changed back to an `ExternalName` and pointed at any pod, ingress-nginx will go back to forwarding like normal and as expected. (this was our fix for a while)
- we are not caching at the ingress-nginx controller. We verified traffic is being forwarded to the incorrect pod via pod access logs.

**What you expected to happen**:

**How to reproduce it**:
We reproduced this on the minimal developer setup on a local kind cluster by deploying via the developer getting started guide (`make dev-env`). 

Then we created two pods running nginx. One will respond to curls with "i am pod1", the other pod will respond with "i am pod2" so we can tell where traffic is being forwarded.

Then we created a service of type `ExternalName` pointing to the IP of pod1. 

Then we created an ingress resource pointing to the service we created above. At this point things behave normally, and ingress-nginx forwards to `pod1` as expected. 

Then we edit our local `/etc/hosts` file to point our fake domain to `127.0.0.1`. This way we can curl the url and have it go straight to the ingress controller running on kind.

Then we edited the service in place, and changed its type from `ExternalName` to `ClusterIP` and pointed the service to pod2 via selector. The Endpoint resource is updated on the cluster with the new endpoint IP like normal (pointing to pod2). If the `ingress-nginx` kubectl plugin is installed, and you view the backend then it appears the backend is pointing at the correct Endpoint as expected. However a curl will show that traffic is being forwarded to the original pod `pod1`

- Here is the service manifest with comments for making it easy to switch back and forth between types

    ```
    apiVersion: v1
    kind: Service
    metadata:
    creationTimestamp: null
    labels:
        app: service1
    name: service1
    namespace: testing
    spec:
    # ports:
    # - name: 80-80
    #   port: 80
    #   protocol: TCP
    #   targetPort: 80
    # selector:
    #   app: pod2
    # type: ClusterIP
    externalName: <name or IP of pod1>
    type: ExternalName
    status:
    loadBalancer: {}
    ```

- Here is the ingress manifest. 

    ```
    apiVersion: networking.k8s.io/v1
    kind: Ingress
    metadata:
    annotations:
        testing: "true"
    name: ingress1
    namespace: testing
    spec:
    ingressClassName: nginx
    rules:
    - host: testing.myfakedomain.com
        http:
        paths:
        - backend:
            service:
                name: service1
                port:
                number: 80
            path: /
            pathType: ImplementationSpecific
    ```

- for convenience here are the pod configurations

    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
    generation: 1
    labels:
        app: pod1
    name: pod1
    namespace: testing
    spec:
    progressDeadlineSeconds: 600
    replicas: 1
    revisionHistoryLimit: 10
    selector:
        matchLabels:
        app: pod1
    strategy:
        rollingUpdate:
        maxSurge: 25%
        maxUnavailable: 25%
        type: RollingUpdate
    template:
        metadata:
        creationTimestamp: null
        labels:
            app: pod1
        spec:
        containers:
        - image: nginx:latest
            imagePullPolicy: Always
            name: pod1
            resources: {}
            terminationMessagePath: /dev/termination-log
            terminationMessagePolicy: File
            lifecycle:
            postStart:
                exec:
                command: ["/bin/sh", "-c", "echo 'i am pod1' > /usr/share/nginx/html/index.html"]
        dnsPolicy: ClusterFirst
        restartPolicy: Always
        schedulerName: default-scheduler
        securityContext: {}
        terminationGracePeriodSeconds: 30
    status: {}
    ---
    apiVersion: apps/v1
    kind: Deployment
    metadata:
    generation: 1
    labels:
        app: pod2
    name: pod2
    namespace: testing
    spec:
    progressDeadlineSeconds: 600
    replicas: 1
    revisionHistoryLimit: 10
    selector:
        matchLabels:
        app: pod2
    strategy:
        rollingUpdate:
        maxSurge: 25%
        maxUnavailable: 25%
        type: RollingUpdate
    template:
        metadata:
        creationTimestamp: null
        labels:
            app: pod2
        spec:
        containers:
        - image: nginx:latest
            imagePullPolicy: Always
            name: pod2
            resources: {}
            terminationMessagePath: /dev/termination-log
            terminationMessagePolicy: File
            lifecycle:
            postStart:
                exec:
                command: ["/bin/sh", "-c", "echo 'i am pod2' > /usr/share/nginx/html/index.html"]
        dnsPolicy: ClusterFirst
        restartPolicy: Always
        schedulerName: default-scheduler
        securityContext: {}
        terminationGracePeriodSeconds: 30
    status: {}
    ```

**Anything else we need to know**:

Our use case is that in production we have a controller that will scale deployments down to 0 pods to sleep until traffic is sent to the application again. When the app "sleeps" the Service changed to ExternalName and pointed to a special "wake up" app that lives off the cluster. When the "wake up" app wants to wake the original app up again, it spins up replicas again and the service is changed back to ClusterIP to point at the app pods.

