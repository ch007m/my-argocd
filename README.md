## ArgoCD issue using AppProject

When an argocd `Application` is installed on a cluster within a user's namespace and where its `AppProject` is also installed within a user's namespace, then we got as error within the `Application` resource status:

`Application referencing project xyz which does not exist`

even if the AppProject is well created under the user's namespace.

Issue: https://github.com/argoproj/argo-cd/issues/21150
Discussion: https://github.com/argoproj/argo-cd/discussions/8402#discussioncomment-11533578

## How to reproduce

- Create a kind cluster and deploy argocd
```bash
idpbuilder create --color --name dabou --port 8443 -c argocd:argocd-cm.yaml
```
- Rollout argocd as the configMap content changed
```bash
kubectl rollout restart -n argocd deployment argocd-server
kubectl rollout restart -n argocd statefulset argocd-application-controller 
```
- Deploy the argocd resources 
```bash
kubectl apply -f app-to-app-error.yaml
```