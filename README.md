# She Code Africa Cloud School Program - Technical Assesment

https://github.com/she-code-africa/Cloud-School-Program-Assessment/blob/main/Assessment.md


## Exercise 1 - Guide on Setting up a CI/CD pipeline with Jenkins

1. Create a VM instance on a cloud platform and install Jenkins on it.
    Refer to this [tutorial](https://docs.microsoft.com/en-us/azure/developer/jenkins/configure-on-linux-vm) for a guide on how-to.
    
2. Create a sample project in your language/framework of choice
    eg. for Java/SpringBoot, we use the [spring initializer](https://start.spring.io/) to setup a maven project
    
3. Create a git repo for your project and push the project files from local to the git repo

4. Install relevant plugins for git and maven (or any appropriate build tool) on the Jenkins - VM Server. [See here](https://www.youtube.com/watch?v=GlQHS7FdVGM) for a guide.

5. Setup a new Jenkins job and add the git link to the SCM tab.

6. Configure the job build tab with the appropriate command. In our case we use maven commands since our sample project is a maven project.

7. Save the job and click on build now to run.

8. To run build automatically as the git repo get updated, consider using github webhooks. [See here](https://blogs.sap.com/2015/12/15/configuring-jenkins-to-run-a-build-automatically-on-code-push/) for a guide on how-to.

9. Now the pipeline run automatically once new changes are pushed to the repo.


## Alternative Solution: Git Actions
- We could achieve similar results with [Git Actions >> Java CI with Maven](https://docs.github.com/en/actions/automating-builds-and-tests/building-and-testing-java-with-maven)
