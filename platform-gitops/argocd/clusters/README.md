# EKS Cluster Registration Notes

This starter assumes self-managed Argo CD on EKS, with Argo CD itself running on a hub EKS cluster.

## Hub cluster auth

Argo CD needs AWS credentials on the hub cluster so `argocd-server`, `argocd-application-controller`, and `argocd-applicationset-controller` can assume the per-cluster role referenced in each cluster secret.

For IRSA, layer in `platform-gitops/argocd/core/values.eks-irsa.example.yaml` after replacing the placeholder management role ARN.

If you prefer EKS Pod Identity, do not use the IRSA overlay. Instead, associate the same management role to those three Argo CD service accounts with `aws eks associate-pod-identity`.

## Managed cluster onboarding

1. Ensure the managed cluster uses an authentication mode that includes the EKS API:

   ```bash
   aws eks update-cluster-config \
     --name use1-apps-prod-a \
     --access-config authenticationMode=API_AND_CONFIG_MAP
   ```

2. Gather the API endpoint and CA bundle that will populate the Argo CD cluster secret:

   ```bash
   aws eks describe-cluster \
     --name use1-apps-prod-a \
     --region us-east-1 \
     --query 'cluster.{endpoint:endpoint,ca:certificateAuthority.data}' \
     --output yaml
   ```

3. Create one IAM role per managed cluster. Its trust policy should allow the Argo CD management role from the hub cluster to assume it.

4. Create an EKS access entry for that per-cluster role and associate an access policy:

   ```bash
   aws eks create-access-entry \
     --cluster-name use1-apps-prod-a \
     --principal-arn arn:aws:iam::111122223333:role/argocd-use1-apps-prod-a \
     --type STANDARD \
     --kubernetes-groups []

   aws eks associate-access-policy \
     --cluster-name use1-apps-prod-a \
     --principal-arn arn:aws:iam::111122223333:role/argocd-use1-apps-prod-a \
     --policy-arn arn:aws:eks::aws:cluster-access-policy/AmazonEKSClusterAdminPolicy \
     --access-scope type=cluster
   ```

5. Update the cluster secret under `argocd/clusters/.../cluster-secret.yaml` with the real:
   - `stringData.server`
   - `awsAuthConfig.clusterName`
   - `awsAuthConfig.roleARN`
   - `tlsClientConfig.caData`

Access Entries are the preferred path. Only fall back to `aws-auth` if you are working with an older cluster setup that has not moved to EKS access entries yet.
