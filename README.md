# mysql-helm-deployment

## Objetivo do desafio
Criar um deployment de MySQL em um cluster Kubernetes utilizando o Helm para gerenciar a instalação e configuração. 
O objetivo é configurar o armazenamento persistente para os dados e expor o serviço para acesso interno no cluster.

## Arquivo values.yaml
```yaml
global:
  storageClass: "standard"
image:
  registry: docker.io
  repository: bitnami/mysql
  tag: 8.0.32
  pullPolicy: IfNotPresent

primary:
  replicaCount: 1
  resources:
    requests:
      cpu: 250m
      memory: 256Mi
    limits:
      cpu: 500m
      memory: 512Mi
  persistence:
    enabled: true
    size: 4Gi
    accessModes:
      - ReadWriteOnce
  containerPorts:
    mysql: 3306
  podLabels:
    app: mysql

  service:
    type: ClusterIP
    ports:
      mysql: 3306

auth:
  rootPassword: "my_root_password123456"
  database: "my_database"
  username: "username"
  password: "password"
```

## Comandos Úteis

Instalação do Helm
```
$ curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
$ chmod 700 get_helm.sh
$ ./get_helm.sh
```

Instalação do chart mysql da bitnami com o uso de valores personalizados por meio do arquivo values.yaml
```
helm install my-mysql bitnami/mysql -f values.yaml
```

Listagem dos pods MySQL que foram criados
```
kubectl get po -l app=mysql
```
Acesso a um pod
```
kubectl exec -it <nome-do-pod> -- bash
```
Conectar-se ao MySQL(Lembrar de utilizar a senha definida em values.yaml)
```
mysql -u username -p
```
Verificar se o banco de dados foi criado
```
SHOW DATABASES;
```
Por fim, executar alguns comandos básicos para finalizar os testes
```
USE my_database;
CREATE TABLE test_table (id INT PRIMARY KEY, name VARCHAR(50));
SHOW TABLES;
```