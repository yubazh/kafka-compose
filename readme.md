prepared docker-compose file to deploy kafka dev-cluster with sasl, ssl and acl.

1. add your ip address to kafka-hosts.txt file
2. start "generate.sh" script 
```
./generate.sh
```
3. start compose with:
```
docker-compose up -d
```
4. to stop, use:
```
docker-compose down
```
or to stop and delete all volumes, use:
```
docker-compose down -v
```
5. grafana: [localhost:3000](localhost:3000) (admin/admin)
web-ui: [localhost:8080](localhost:8080) (admin/admin)
6. useful commands:
```
# topic creation
./kafka-topics.sh --bootstrap-server 192.168.0.188:29092 --create --topic test-topic --partitions 3 --replication-factor 3 --command-config admin.properties

# create ACL for topic test-topic
./kafka-acls.sh --bootstrap-server 192.168.0.188:29092 --add --allow-principal User:da --operation All --group '*' --topic test-topic --command-config admin.properties

# checking ACLs
./kafka-acls.sh --bootstrap-server 192.168.0.188:29092 --list --topic test-topic --command-config admin.properties

# starting producer
./kafka-console-producer.sh --bootstrap-server 192.168.0.188:29092 --topic test-topic --producer.config client.properties.da

# starting consumer
./kafka-console-consumer.sh --bootstrap-server 192.168.0.188:29092 --topic test-topic --consumer.config client.properties.da  --from-beginning
```
7. client credentials is in KAFKA_CLIENT_USERS and KAFKA_CLIENT_PASSWORD envs
8. client and admin properties example is in admin.properties and client.properties.da files

Links:
- https://jaehyeon.me/blog/2023-07-20-kafka-development-with-docker-part-11/
- https://medium.com/@penkov.vladimir/kafka-cluster-with-ui-and-metrics-easy-setup-d12d1b94eccf
- https://github.com/bitnami/containers/blob/main/bitnami/kafka/README.md