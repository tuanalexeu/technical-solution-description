<h1 align="center">
<img src="https://raw.githubusercontent.com/peaceiris/mkdocs-material-boilerplate/master/docs_sample/images/graduate-cap.png" alt="MkDocs icon" width="170">
<br>Technical Solution Description (TSD) for Logiweb Microservices
</h1>

![Eyecatch image of MkDocs Material Boilerplate (Starter Kit)](https://raw.githubusercontent.com/peaceiris/mkdocs-material-boilerplate/master/docs_sample/images/material.png)


## Description

<p>
Here you can read how and in what way the project was written, built and deployed to GKE.
</p>

<!-- https://shields.io/ -->


## Success criteria
1. Functionality works (UI is required)
2. Maven-based project, split into modules (build with one command, deploy with one command)
3. The interfaces of the subject area are described.
4. MySQL DB is connected
5. The entities of the subject area have been created; mapping to tables in the database
6. Working with entities through DAO
7. The application is deployed to AS
8. Implemented exception handling
9. Logging is enabled
10. Technical solution description
11. Unit tests for business logic

## About this site

Instead of writing TSD the old way (Such as PDF/Word etc.), 
I decided to write using modern tools. My eyes fell on MK-Docs tool. 
MK-Docs is used to create documentation site with minimal efforts. 

All I have to do is to define folder structure and fill it with MD files, like follows:
```yaml
# Page tree
nav:
  - 'Home': 'index.md'
  - 'Microservices':
      - 'Logiweb': 'microservices/logiweb-service.md'
      - 'Client Service': 'microservices/client-order-service.md'
      - 'Dashboard': 'microservices/info-table.md'
      - 'Spring Cloud Config': 'microservices/config-service.md'
  - 'Logging': 'logging.md'
  - 'Testing': 'testing.md'
  - 'MQ Broker': 'rabbitmq.md'
  - 'Deployment':
      - 'GitLab': 'deployment/gitlab.md'
      - 'Docker': 'deployment/docker.md'
      - 'Kubernetes': 'deployment/k8s.md'
      - 'Google Cloud Platform': 'deployment/gcp.md'
  - 'Summary': 'summary.md'
```


## Links

- [tuanalexeu/gmail: Get in touch with me]
- [tuanalexeu/logiweb-microservices: GitLab]
- [tuanalexeu/dockerhub: DockerHub]

[tuanalexeu/gmail: Get in touch with me]: mailto:alekseytyan45@gmail.com
[tuanalexeu/logiweb-microservices: GitLab]: https://gitlab.com/tuanalexeu/logiweb-microservices
[tuanalexeu/dockerhub: DockerHub]: https://hub.docker.com/u/tuanalexeu
