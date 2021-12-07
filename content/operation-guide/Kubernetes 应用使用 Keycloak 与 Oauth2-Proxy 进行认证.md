+++
title = "使用 Keycloak 与 Oauth2-Proxy 认证 Kubernetes 应用"
description = ""
weight = 3
+++

## 安装 Keycloak-Operator

```bash
# 1. 创建 keycloak 命名空间
kubectl create ns keycloak

# 2. 创建 serviceaccount
kubectl -n keycloak apply -f https://raw.githubusercontent.com/keycloak/keycloak-operator/master/deploy/service_account.yaml

# 3. 创建 Role 和 Rolebinding（Namspace 范围）
kubectl -n keycloak apply -f https://raw.githubusercontent.com/keycloak/keycloak-operator/master/deploy/role.yaml
kubectl -n keycloak apply -f https://raw.githubusercontent.com/keycloak/keycloak-operator/master/deploy/role_binding.yaml

# 这里创建的 keycloak-operator 为 namespace 范围，operator 只作用于 keycloak 这个 namespace 下。
# 如果需要 cluster 范围，需要改用 ClusterRole 和 ClusterRoleBinding
# kubectl -n keycloak apply -f https://raw.githubusercontent.com/keycloak/keycloak-operator/master/deploy/cluster_roles/cluster_role.yaml
# kubectl -n keycloak apply -f https://raw.githubusercontent.com/keycloak/keycloak-operator/master/deploy/cluster_roles/cluster_role_binding.yaml


# 3. 安装 crds
kubectl apply -f https://raw.githubusercontent.com/keycloak/keycloak-operator/master/deploy/crds/keycloak.org_keycloaks_crd.yaml
kubectl apply -f https://raw.githubusercontent.com/keycloak/keycloak-operator/master/deploy/crds/keycloak.org_keycloakusers_crd.yaml
kubectl apply -f https://raw.githubusercontent.com/keycloak/keycloak-operator/master/deploy/crds/keycloak.org_keycloakrealms_crd.yaml
kubectl apply -f https://raw.githubusercontent.com/keycloak/keycloak-operator/master/deploy/crds/keycloak.org_keycloakclients_crd.yaml
kubectl apply -f https://raw.githubusercontent.com/keycloak/keycloak-operator/master/deploy/crds/keycloak.org_keycloakbackups_crd.yaml

# 安装 keycloak-operator
kubectl -n keycloak apply -f https://raw.githubusercontent.com/keycloak/keycloak-operator/master/deploy/operator.yaml
```