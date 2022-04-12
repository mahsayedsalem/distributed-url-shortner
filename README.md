<h1 align="center">
  Shortening
</h1>

<h4 align="center">Distributed URL Shortner</h4>

<p align="center">
  <a href="#key-features">Key Features</a> •
  <a href="#targer-design">Target Design</a> •
  <a href="#stack-decisions">Stack Decisions</a>
</p>

## Key Features

* User Profile 👤 - Keep your shortened URLs in your profile.
* Analytics&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;📈 - Click-based Statistics on all your URLs.
* Distributed&nbsp;  🚀 - Can handle huge number of reads/writes.
* Caching &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;🏪 - Cache the 20% most visited shortened URLs.

## Target Design

![ScreenShot](/images/url-shortner-diagram.png)

## Stack Decisions
* Java/Spring Framework as a primary development language.
* MongoDB as a source of truth since it's Optimized for reads. Our read/write ratio could reach 200/1. 
* Redis as a Caching Database. We will cache 20% of the most visited URLs.
* We will publish to Kafka on every visit: {SHORT-URL,  URL, USER}. These info are going to be aggregated using KSQL.
* Implement CQRS on the Critical Services (Redirector - Convertor - Generator).
* Docker to containerize our services. 
* Kubernetes for orchestration, secrets, discovery, load balancing and configs. 
* Docker-compose for services development.
* Github Actions as our main CI/CD Tool. 
* Terraform as IaaC. 
* AWS ECS as main Cloud Provider. 
