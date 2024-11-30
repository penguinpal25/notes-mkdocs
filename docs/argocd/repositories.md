## Private Git Repos

- Public repos can be used directly in application.
- Private repos needs to be registered in ArgoCD with proper authentication before using it in applications.
- ArgoCD support connecting to private repos using below ways:
  - HTTPs: using username and password or access token.
  - SSH: using ssh private key.
  - GitHub / GitHub Enterprise : GitHub App credentials.
- Private repos credentials are stored in normal k8s secrets.
- You can register repos using declarative approach, cli and web UI.

## Git Repo using HTTPs
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: private-repo
  namespace: argocd
  labels:
    argocd.argoproj.io/secret-type: repository
stringData:
  type: git
  url: https://github.com/argoproj/private-repo
  password: my-password
  username: my-username
```
## Git Repo using HTTPs - insecure 
- To ignore the SSL/TLS certificate
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: private-repo
  namespace: argocd
  labels:
    argocd.argoproj.io/secret-type: repository
stringData:
  type: git
  url: https://github.com/argoproj/private-repo
  password: my-password
  username: my-username
  insecure: true
```
## Git Repo using HTTPs and Tls
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: private-repo
  namespace: argocd
  labels:
    argocd.argoproj.io/secret-type: repository
stringData:
  type: git
  url: https://github.com/argoproj/private-repo
  password: my-password
  username: my-username
  tlsClientCertData: ….
  tlsClientCertKey: ….
```
## Git Repo using SSH
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: private-repo
  namespace: argocd
  labels:
    argocd.argoproj.io/secret-type: repository
stringData:
  type: git
  url: https://github.com/argoproj/private-repo
  sshPrivateKey: |
    -----BEGIN OPENSSH PRIVATE KEY-----
    ...
    -----END OPENSSH PRIVATE KEY-----
```
## Git Repo using GitHub App
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: private-repo
  namespace: argocd
  labels:
    argocd.argoproj.io/secret-type: repository
stringData:
  type: git
  url: https://github.com/argoproj/private-repo
 githubAppID: 1
 githubAppInstallationID: 2
 githubAppPrivateKey: |
   -----BEGIN OPENSSH PRIVATE KEY-----
   ...
   -----END OPENSSH PRIVATE KEY-----
```
## Private Helm repo
- Public standard Helm repos can be used directly in application.
- Non standard Helm repositories have to be registered explicitly.
- Private Helm repos needs to be registered in ArgoCD with proper authentication before using it in applications.
- ArgoCD support connecting to private Helm repos using username/password and tls cert/key.
- Registering Helm repos in ArgoCD can be done declaratively , CLI and Web UI.

### Private Helm Repo - declaratively
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: private-repo
  namespace: argocd
  labels:
    argocd.argoproj.io/secret-type: repository
stringData:
  type: helm
  url: https://argoproj.github.io/argo-helm
  password: my-password
  username: my-username
  tlsClientCertData: ….
  tlsClientCertKey: ….
```
### Private Helm Repo - CLI (Add a private Helm repository named 'stable' via HTTPS)
```bash
argocd repo add https://charts.helm.sh/stable --type helm --name stable --username test --password test
```
## Credential Templates
- Used If you want to use the same credentials for multiple repositories in your organization without having to repeat credential configuration.
- Defined as same as repositories credentials information, with different label value “argocd.argoproj.io/secret-type: repo-creds”.
- In order for ArgoCD to use a credential template for any given repository, the following conditions must be met:
  - The URL configured for a credential template (e.g. https://github.com/vyshnavlal ) must match as prefix for the repository URL (e.g. https://github.com/vyshnavlal/argocd-example-apps).
  - The repository must either not be configured at all, or if configured, must not contain any credential information.
- Registering credentials in ArgoCD can be done declaratively , CLI and Web UI.

### Credential Templates - declaratively
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: private-repo-creds
  namespace: argocd
  labels:
    argocd.argoproj.io/secret-type: repo-creds
stringData:
  type: git
  url: https://github.com/vyshnavlal
  password: my-password
  username: my-username
```
### Credential Templates - CLI
- Add credentials with user/pass authentication to use for all repositories under https://git.example.com/repos
  ```bash
  argocd repocreds add https://git.example.com/repos/ --username git - -password secret
  ```
