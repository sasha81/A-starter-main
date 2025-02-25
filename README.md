# A starter for a distributed multi-language data science & information system
Here are instructions to build and launch the starter, described in [this paper](https://dzone.com/articles/a-starter-for-a-distributed-multi-language-analyti). Most of the creds/configs are in the docker-compose.yaml file.
## Prerequisites
* Rabbit MQ:
    + First, run `docker compose up rabbit-mq`.
    + Then, create a new user (default user will not do).
    + Finally, with the new user creds, login to the [admin menu](http://localhost:15672) to create the following exchanges, routing keys->(binded to) queues:
        - default `aux_exchange`, `aux_routingkey` -> `aux_queue`; the queue for the Spring Boot service rpc calls,
        - `aux_exchange`, `nest-event-routingkey` -> `nest-event-queue`; the queue for the NestJS users service domain events (new user created, user updated), 
        - `aux_exchange`, `nest-routingkey` -> `nest-front-back`; the queue to call the NestJS users service rpc (get all users),
        - `aux_exchange`, `python-routingkey` -> `python-queue`; the queue to call the Python service rpc (get all users),
        - `aux_exchange`, `ws-in` -> `ws-in-routingkey`; the queue for messages to the NestJS WS service,
        - `aux_exchange`, `ws-out` -> `ws-out-routingkey`; the queue for messages from the NestJS WS service.
* Mongo DB: 
    + First, run `docker compose up mongodb`.
    + Then, login to the mongodb server with a `MONGO_INITDB_ROOT_USERNAME/PASSWORD` specified in the `services/mongodb` of the docker-compose.yaml.
    + Finally, create the following databases:
        - `NEST` - for the NestJS users service aggregate,
        - `NEST_VIEW` - for the NestJS users service view, 
        - `testdb` - for the Python service.
* PostgreSQL:
    + Run `docker compose up postgresdb`. A data base, specified in `POSTGRES_DB`, is created.
    
* Eureka:
    + First, in Intellij Idea, open the A-starter-java project.
    + Then, go to the Gradle tab, then run `A-starter-java -> eureka -> Tasks -> build -> bootJar`.
    + Finally, run `docker compose up eureka-server`.
* Gateway:
    + First, in Intellij Idea, open the A-starter-java project.
    + Then, go to the Gradle tab, then run `A-starter-java -> gateway -> Tasks -> build -> bootJar`.
    + Finally, run `docker compose up gateway-server`.

## Github
The starter uses github submodules to link A-starter-typescript, A-starter-python, A-starter-java (submodules) to A-starter-main repository.
So, to pull the main module and its submodules, run `git pull --recurse-submodules`.

## Miscroservices
See the README.md files of the A-starter-typescript, A-starter-python, A-starter-java projects to run the domain, data science, and composer micro-services.