# sup

do these steps to reproduce the issue

1. do a `make dev-env` to make the kind cluster. the ingress-nginx controller should be running by default on the `ingress-nginx` namespace. 
2. make the test namespace `kubectl create namespace testing`
3. create the deployments/pods `kubectl -n testing apply -f pods.yaml`
4. create the service (initally points to externalname "www.google.com") `kubectl -n testing apply -f service.yaml`
5. create the ingress `kubectl -n testing apply -f ingress.yaml`
6. Boom the test set up is done. check out the `testing` namespace. `kubectl -n testing get all,ing -o wide`

```
✗ kc -n testing get all,ing -o wide
NAME                        READY   STATUS    RESTARTS   AGE     IP           NODE                              NOMINATED NODE   READINESS GATES
pod/pod1-758f994ccb-nfpg8   1/1     Running   0          4h18m   10.244.0.8   ingress-nginx-dev-control-plane   <none>           <none>

NAME               TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE     SELECTOR
service/service1   ClusterIP   10.96.223.229   <none>        80/TCP    4h18m   app=pod1

NAME                   READY   UP-TO-DATE   AVAILABLE   AGE     CONTAINERS   IMAGES         SELECTOR
deployment.apps/pod1   1/1     1            1           4h18m   pod1         nginx:latest   app=pod1

NAME                              DESIRED   CURRENT   READY   AGE     CONTAINERS   IMAGES         SELECTOR
replicaset.apps/pod1-758f994ccb   1         1         1       4h18m   pod1         nginx:latest   app=pod1,pod-template-hash=758f994ccb

NAME                                 CLASS   HOSTS            ADDRESS         PORTS   AGE
ingress.networking.k8s.io/ingress1   nginx   www.google.com   10.96.129.124   80      4h18m
```

7. curl localhost port 80, and the ingress controller should forward your request to google.com 

```
curl -H "Host: www.google.com" http://127.0.0.1/
<!doctype html><html itemscope="" itemtype="http://schema.org/WebPage" lang="en"><head><meta content="Search the world's information, including webpages, images, videos and more. Google has many special features to help you find exactly what you're looking for." name="description"><meta content="noodp" name="robots"><meta c
## truncate a bunch of html body response. you get it.
```

8. This is the step that causes the bug. Change the servie type from `ExternalName` to `ClusterIP`:

```
kubectl -n testing edit service service1

### change the spec of the service to the following:
spec:
  ports:
  - name: 80-80
    port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: pod1
  type: ClusterIP

##
```

9. Your stuff is now broken. Curls will still be sent to the ExternalName stuff. 

10. A reload of nginx in the controller pod fixes the issue. `kubectl -n ingress-nginx exec -it <controller pod name> -- nginx -s reload`
