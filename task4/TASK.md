# Задание 4. Защита доступа к кластеру Kubernetes

   1.`rbac-all.yaml` - Полный набор манифестов Kubernetes RBAC: `ClusterRole`, `Role`, `ClusterRoleBinding`, `RoleBinding`.
  2. `Roles.md` - Описание ролей, их прав и связанных с ними групп пользователей.

## Инструкция по выпролнению

1. Примените манифесты:

   ```bash
   kubectl apply -f rbac-all.yaml
   ```

2. Убедитесь, что ресурсы создались успешно:

   ```bash
   kubectl get clusterroles cluster-admin cluster-viewer
   kubectl get roles -n default pod-admin
   kubectl get clusterrolebindings admins-cluster-admin viewers-cluster-viewer
   kubectl get rolebindings -n default dev-pod-admin
   ```

## Проверка прав

Пример проверки того, какие операции разрешены для той или иной группы пользователей:

```bash
# Проверяем, может ли группа dev создавать Pod-ы в default
kubectl auth can-i create pods \
  --as-group=dev \
  -n default

# Проверяем, может ли группа viewers получить список Pod-ов во всём кластере
kubectl auth can-i list pods \
  --as-group=viewers
```