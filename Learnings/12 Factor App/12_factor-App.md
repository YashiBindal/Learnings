## 1. Codebase

- Git: to create shared codebase and github as code repo
- aws ECR

## 2. Dependencies
- install dependencies explicitlyL requirement.txt files
- make sure to use proper version for dependencies
- isolate dependencies: different apps req different version of a dependency: user virtual environments
- docker: best approach to install dependencies and create different environments

## 3. Concurrency

- horizontal scaling
- use of load balancers

## 4. Process

- 12 factor process are stateless n share nothing: don't use internal memory for processes, sticky session are a violation 
- store everything in external db like redis or any other db

## 5. Backing services

- attached services like redis, smtp, etc should be configured as such that we don't need to update our code based on their hosted env

## 6. Config
- store variables in separate config files like .env for python

## 7. Build, release, Run

- build, release and run must be separate
- build + config files make a release
- Run same code in different env, and rollback to previous versions easily

## 8. Port Binding

- to expose serves for access

## 9. Disposability

- can start or stop ata moments notice
- should shut down gracefully using SIGTERM signal and SIGKILL signal, prevents process shutdown if a process already running, will shutdwon only after task ends

## 10. Dev Prod Parity

- dev and prod env should be similar, to reduce prod deploy time
- avoid using diff backend services for diff env
- use CI/CD tools to reduce production time

## 11. Logs
- monitoring errors n issues
- logs should not be local
- tightly coupled central logs is discouraged
- all process should create their own local logs which then should be transfered to a central logging solution for analytics

## 12. Admin Processes

- admin tasks should be separate and should be run on a different identical setup

