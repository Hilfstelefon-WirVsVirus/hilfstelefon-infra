# hilfstelefon infrastructure documentation
This project documents AWS infrastructure of the `hilfstelefon` project. The project was created during the [#WirVsVirus Hackathon](https://devpost.com/software/hilfstelefon).

## General AWS services overview
* S3
* CloudFront
* ECS, ECR
* Network Load Balancer
* Route 53
* RDS (PostgreSQL)

## S3
The [frontend project](https://github.com/Hilfstelefon-WirVsVirus/hilfstelefon-frontend) is hosted on a S3 bucket. The bucket is exposed publicly via CloudFront. Assets are deployed via github actions, triggered on release creation.

## CloudFront
A CloudFront instance listens on [hilfstelefon.de](hilfstelefon.de) and [www.hilfstelefon.de](www.hilfstelefon.de) and forwards to the S3 bucket internally. It provides and forces https.

## ECS, ECR
ECS is used as the container orchestrator for the [backend project](https://github.com/Hilfstelefon-WirVsVirus/hilfstelefon-backend). It uses ECR as the container registry and Fargate as the serverless container engine. Deployment is triggered via github action on release creation.

## Network Load Balancer
A Network Load Balancer is used to forward external traffic to the container service. It provides and forces TLS.

## Route 53
We use Route 53 to manage the hilfstelefon.de zone. We set an alias record for [hilfstelefon.de](hilfstelefon.de) pointing at the CloudFront instance and an alias record for [api.hilfstelefon.de](api.hilfstelefon.de) pointing at the Network Load Balancer.

## RDS (PostgreSQL)
RDS is used to provide a PostgreSQL database instance. It is only accessible internally. Host and login credentials are stored in the Secret Manager to be fetched during the deployment process.