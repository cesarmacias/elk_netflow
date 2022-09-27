# **elk_netflow**

### Instalacion y configuracion de Elastic Stack 8.X para recibir paquetes netflow en Ubuntu 22.04
-------------
## **Instalacion de Elasticsearch**

1. Instalacion de prerequisitos
    ```
    sudo apt install curl wget apt-transport-https default-jdk gnupg -y
    ```
2. Agregar el respositorio de Elasticsearch 8.X
    ```
    echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-8.x.list
    ```
3. Agregar el cartificado del repositorio
    ```
    wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg
    ```
4. Instalacion de Elasticsearch
    ```
    sudo apt update && sudo apt install elasticsearch -y
    ```
5. Copiar la calve del usuario 'elastic'
6. Habilitar servicio para inicio en boot
    ``` 
    sudo systemctl daemon-reload
    sudo systemctl enable elasticsearch
    ```
7. Configurar el elasticsearch.yml - Lineas que se deben ediar y agregar
    ```
    sudo vi /etc/elasticsearch/elasticsearch.yml 
    ```
    > cluster.initial_master_nodes: ["node-1"]
    >
    > action.auto_create_index: .monitoring*,.watches,.triggered_watches,.watcher-history*,.ml*
    >
    > network.host: 0.0.0.0
    >
    > node.name: node-1
    >
    > cluster.name: app-dev
8. Configurar /etc/hosts con "node-1"
    ```
    sudo vi /etc/hosts
    ```
    > 127.0.1.1	node-1
9. Iniciar el servicio de elasticsearch
    ```
    sudo systemctl start elasticsearch
    ```
10. Validar estado del servicio elasticsearch
    ```
    sudo curl -k --cacert /etc/elasticsearch/certs/http_ca.crt -u elastic https://localhost:9200/_cluster/health?preatty
    ```

    ![Estado de cluster](/imagenes/elastic-health.png)

-------------
## **Instalacion de Kibana**
