# Задание 5. Управление трафиком внутри кластера Kubernetes

   1. `non-admin-api-allow.yaml` - Набор `NetworkPolicy`, который разрешает обмен трафиком между обычным front-end (`front-end-app`) и back-end API (`back-end-api-app`), а также изолирует сервис `admin-backend-api-app` от обращений не-администраторских Pod-ов.

## Порядок выполнения

1. Примените сетевые политики:

   ```bash
   kubectl apply -f non-admin-api-allow.yaml
   ```

2. Проверьте, что обычный front-end может обращаться к back-end API:

   ```bash
   kubectl exec -it front-end-app -- curl --connect-timeout 5 http://back-end-api-app
   ```
   Ожидаемый результат: HTTP-ответ (код 200 / 404 и т.д.), то есть соединение установлено.

3. Проверьте, что тот же front-end НЕ может обратиться к административному back-end API:

   ```bash
   kubectl exec -it front-end-app -- curl --connect-timeout 5 http://admin-backend-api-app
   ```
   Ожидаемый результат: таймаут подключения или сообщение "Connection refused".

4. (Дополнительно) Убедитесь, что административный front-end (`admin-front-end-app`) по-прежнему может обращаться к своему back-end:

   ```bash
   kubectl exec -it admin-front-end-app -- curl --connect-timeout 5 http://admin-backend-api-app
   ```

