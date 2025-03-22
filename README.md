# ArgoCD and Flux Demo

This repository sets up an Amazon EKS cluster with ArgoCD and Flux for GitOps-based continuous deployment, using Ansible
for bootstrapping and GitHub Actions for the CI/CD.

## Overview

**Goal**: Automate the deployment of ArgoCD and Flux on an EKS cluster, with application manifests managed via GitOps.

**Tools used**:

* **Ansible**: Installs ArgoCD (via Helm) and Flux (via CLI).
* **GitHub Actions**: Runs Ansible playbooks and triggers Flux synchronization.
* **ArgoCD**: Manages application deployments declaratively.
* **Flux**: Syncs the repository with the cluster.

### Prerequisites (for this demo)

1. An existing EKS cluster (manually created).
2. AWS IAM user configured with access to the cluster.
3. GitHub repository with Secrets configured.
4. `kubectl` configured to interact with your EKS cluster.

---

### Templating Details

- Variables set in `./clusters/eks-cluster/argocd.yml` are resolved by Ansible.
- Ansible renders them into the manifest at runtime.

### Verification

- Check ArgoCD: `kubectl get pods -n argocd`.
- Check Flux: `kubectl get pods -n flux-system`.
- Access ArgoCD UI: `kubectl port-forward svc/argocd-server -n argocd 8080:443`.

### Customization

- Modify `./clusters/*/argocd.yml` to deploy your apps.
- Adjust `./clusters/*/flux-kustomization.yml` for additional paths or settings.
- Extend Ansible roles for more cluster configurations.

### Troubleshooting

- **Pipeline fails**: Check GitHub Actions logs for errors (e.g. missing Secrets).
- **ArgoCD/Flux not running**: Verify EKS cluster access and AWS credentials.
- **Sync issues**: Ensure that `GITHUB_TOKEN` has the correct permissions.

---

## Repository file tree

```
├── .github/workflows/
│   ├── deploy.yml
│
├── ansible/
│   ├── playbook.yml
│   ├── vars.yml
│   ├── roles/
│   │   ├── argocd/
│   │   │   ├── tasks/
│   │   │   │   ├── main.yml
│   │   ├── flux/
│   │   │   ├── tasks/
│   │   │   │   ├── main.yml
│
├── clusters/
│   ├── eks-cluster/
│   │   ├── argocd.yml
│   │   ├── flux-kustomization.yml
│
├── README.md
```
