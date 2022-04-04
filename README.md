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

10. To create a simple pipeline for our app, we add a new item to jenkins server and select the pipeline option.

11. And use the pipeline script below:

```
// All valid declarative pipelines must be enclosed within a pipeline block
pipeline {

    // specifies where the pipeline would execute in the jenkins environment
    agent any

    // defining tools to auto-install and put on the PATH
    tools {
        // Install the Maven version configured as "MAVEN_HOME" and add it to the path.
        maven "MAVEN_HOME"
    }

    // Specifies different segment of the pipeline. Containing a sequence of one or more stage directives
    stages {
    	stage('Source') {
		// Get the code from a GitHub repository
		steps {
		    git branch: 'main', url: 'https://github.com/zee467/SCA-Cloud-School-Application.git'
		}
            
	}
		
	stage('Static Analysis') {
		// check code style
		steps {
			sh "mvn checkstyle:checkstyle"
		}
        }
        
        stage('Test') {
            steps {
                sh "mvn test -Dmaven.test.failure.ignore=true"
            }
            
            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
		
        stage('Build') {
            steps {
                // Run Maven on a Unix agent.
                sh "mvn clean install"
            }
        }
        
        stage("Staging") {
            steps{
                withEnv(['JENKINS_NODE_COOKIE=dontkill']) {
                    sh 'nohup ./mvnw spring-boot:run -Dserver.port=8989 &'
                }
            }
                
        }
    }
}
```


## Alternative Solution: Git Actions
- We could achieve similar results with [Git Actions >> Java CI with Maven](https://docs.github.com/en/actions/automating-builds-and-tests/building-and-testing-java-with-maven)


## Resources
- [Pipeline Script](https://www.jenkins.io/doc/book/pipeline/syntax/)
- [About CI/CD](https://www.guru99.com/ci-cd-pipeline.html)
- [Maven Lifecycle](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#Lifecycle_Reference)
