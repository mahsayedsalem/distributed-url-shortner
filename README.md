<h1 align="center">
  Shortening
</h1>

<h4 align="center">Distributed URL Shortener</h4>

<p align="center">
  <a href="#target-key-features">Target Key Features</a> •
  <a href="#target-architecture">Target Architecture</a> •
  <a href="#design-decisions">Design Decisions</a> •
</p>

## Target Key Features

* User Profile 👤 - Keep your shortened URLs in your profile.
* Analytics&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;📈 - Click-based Statistics on all your URLs.
* Distributed&nbsp;  🚀 - Can handle huge number of reads/writes.
* Caching &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;🏪 - Cache the 20% most visited shortened URLs.

## Target Architecture

![ScreenShot](/images/arch-v1.png)

## Design Decisions

### Technologies

* Java/Spring Framework as a primary development language.
* MongoDB as a source of truth since it's optimized for reads. Our read/write ratio could reach 200/1. 
* Redis as a Caching Database. We will cache 20% of the most visited URLs.
* We will publish to Kafka on every redirect: {SHORT-URL,  URL, USER}. These messages are going to be aggregated using KSQL.
* Implement CQRS on the Critical Services (Redirector - Convertor - Generator).
* Docker to containerize our services. 
* Kubernetes for orchestration, secrets, discovery, load balancing and configs. 
* Docker-compose for services development.
* ELK stack for Logs Management.
* Prometheus for Monitoring and Alerting.
* Github Actions as our main CI/CD Tool. 
* Terraform as IaaC.
* AWS ECS as a main Cloud Provider.

### Key Generation Service
* Key Generation Service (Generator) generates random six-letter strings beforehand and stores them in a database.
* Whenever we want to shorten a URL, the Convertor will call the Genartor and take one of the already-generated keys and use it.
* The Generator will use two tables to store keys: one for keys that are not used yet, and one for all the used keys.
* The Generator will always push some Keys to a Kafka topic, where Convertor instances will consume from.

### Cache Eviction Policy
Least Recently Used (LRU) is going to be the eviction policy for our system. Under this policy, we discard the least recently used URL first.
