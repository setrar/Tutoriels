# Dashboard

- [ ] Deploy Dashboard

```
$ kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.2.0/aio/deploy/recommended.yaml
```

- [ ] Get status

```
$  kubectl get deploy/kubernetes-dashboard --namespace kubernetes-dashboard
NAME                   READY   UP-TO-DATE   AVAILABLE   AGE
kubernetes-dashboard   1/1     1            1           22h
```

- [ ] Create [sample user](https://github.com/kubernetes/dashboard/blob/master/docs/user/access-control/creating-sample-user.md) service account

Since the dashboard shows us resources across our whole cluster, we will need to create an admin account `admin-user` for it.

```yaml
$ kubectl apply -f - <<EOF
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kubernetes-dashboard
EOF
```

- [ ] Create its `role`

```yaml
$ kubectl apply -f - <<EOF
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kubernetes-dashboard
EOF
```

:bookmark: Command line proxy

You can access Dashboard using the `kubectl` command-line tool by running the following command:

```
$ kubectl proxy
```

:star: Create a tunnel proxy from your PC to access the dashboard remotely

```
$ ssh -L 8001:127.0.0.1:8001 ubuntu@orion
ubuntu@orion's password: <enter passwd>

# on the control plane server
ubuntu@orion:~$ kubectl proxy &
```

:x: if issue

```
$ netstat -lnp | grep 8001
# kill the process
```


`Kubectl` will make Dashboard available at `http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy`

- [ ] Let's get the [bearer token](https://github.com/kubernetes/dashboard/blob/master/docs/user/access-control/creating-sample-user.md#getting-a-bearer-token)

```
$ kubectl -n kubernetes-dashboard get secret \
   $(kubectl -n kubernetes-dashboard get sa/admin-user --output jsonpath="{.secrets[0].name}") \
   --output go-template="{{.data.token | base64decode}}" \
   && echo ''
 ```
 
 Now copy the token and paste it into `Enter token` field on the login screen.
 