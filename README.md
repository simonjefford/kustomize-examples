# Config map generator example

1. Run `kustomize build`. Note the name of the generated config map and it's reference in the deployment.

``` yaml
apiVersion: v1
data:
  test.properties: |
    foo=bar
kind: ConfigMap
metadata:
  name: example-k9ctt2kg22
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - image: nginx:1.14.2
        name: nginx
        ports:
        - containerPort: 80
        volumeMounts:
        - mountPath: /etc/foo
          name: example
          readOnly: true
      volumes:
      - configMap:
          name: example-k9ctt2kg22
        name: example
```

2. Make a change to the `test.properties` file mentioned by `configMapGenerator`

```
foo=baz
```

3. Rerun kustomize build. Note the config map name changes, along with its reference

``` yaml
apiVersion: v1
data:
  test.properties: |
    foo=baz
kind: ConfigMap
metadata:
  name: example-ktm5tk6ct7
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - image: nginx:1.14.2
        name: nginx
        ports:
        - containerPort: 80
        volumeMounts:
        - mountPath: /etc/foo
          name: example
          readOnly: true
      volumes:
      - configMap:
          name: example-ktm5tk6ct7
        name: example
```
