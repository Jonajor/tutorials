Deploy AWS resources in localstack with Terraform
=================================================

[Detailed tutorial here.](https://baptiste.bouchereau.pro/tutorial/automatic-provisioning-of-localstack/)

An example on how to use docker events to automatically deploy localstack resources that mock AWS services. The following instructions focus on how to deploy:
* a dynamodb table
* a lambda reading data and putting data to this table

Usage
-----

Run

```bash
git clone https://github.com/Ovski4/tutorials.git
cd localstack-part-3
docker network create localstack-tutorial
docker-compose up -d
docker-compose logs -f docker-events-listener
```

Wait for the resources to be deployed, then invoke the lambda multiple times and scan the table to see new items and their counters being incremented:

```bash
docker-compose exec docker-events-listener aws lambda invoke --function-name counter --endpoint-url=http://localstack:4574 --payload '{"id": "test"}' output.txt
docker-compose exec docker-events-listener aws dynamodb scan --endpoint-url http://localstack:4569 --table-name table_1

docker-compose exec docker-events-listener aws lambda invoke --function-name counter --endpoint-url=http://localstack:4574 --payload '{"id": "test2"}' output.txt
docker-compose exec docker-events-listener aws dynamodb scan --endpoint-url http://localstack:4569 --table-name table_1
```