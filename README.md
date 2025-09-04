1.Secure Maven CI/CD Pipeline with Docker, Trivy, and Ansible

2.Project Overview
    This project demonstrates a full CI/CD pipeline of a Maven-based Java application.  
    It showcases real-world DevOps practices, including automated build, security scanning, containerization, and deployment.

3.With this project, you can see:  
    - Maven build automation (validate, compile, package)  
    - Docker image creation and pushing to Docker Hub  
    - Security scanning using **Trivy** for high and critical vulnerabilities  
    - Deployment to local and production servers using **Ansible**  
    - Jenkins pipeline orchestration with manual approval for production  
    - Email notifications for pipeline success or failure
  

4.The CI/CD flow looks like this:
   Jenkins Pipeline → Maven Build → Docker Image Build → Trivy Scan → Docker Push → Deployment via Ansible

5.
    - **Local deployment:** Runs the container on **port 8096**  
    - **Production deployment:** Deployed via Ansible on **port 8095**

6. Prerequisites
    Before running this pipeline, make sure you have:  
    - Jenkins installed and configured  
    - Docker installed on Jenkins and remote nodes  
    - Ansible installed (either system-wide or in a virtual environment)  
    - Java and Maven installed  
    - Git
  


    
