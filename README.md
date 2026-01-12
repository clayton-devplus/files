# files



      - name: Upgrade do DBeaver usando values.yaml
        run: |
          helm upgrade --install cloudbeaver avisto/cloudbeaver \
            --namespace $NAMESPACE \
            --create-namespace \
            --values $K8S_OVERLAY_DIR/values.yaml

      # - name: Aguardar rollout do RabbitMQ
      #   run: |
      #     kubectl rollout status statefulset/rabbitmq-server -n $NAMESPACE --timeout=2m
      - name: Configurar Secret
        run: |
          kubectl apply -f $K8S_OVERLAY_DIR/secret.yaml -n $NAMESPACE
          kubectl apply -f $K8S_OVERLAY_DIR/auth-secret.yaml -n $NAMESPACE

      - name: Verificar Deploy e Pods
        run: |
          kubectl get sts,svc -n $NAMESPACE
          kubectl get pods -n $NAMESPACE -o wide
