pipeline {
    agent {
        label "frontend"
    }

    stages { 
        stage("checkout scm") {
            steps {
                git url:"https://github.com/prateekkumawat/jenkins-830-project.git", branch: "master"
            }
        }
        stage("build Image") { 
            steps {
                sh "docker build -t jenkinstest ."
            }
        }
        stage("push Image") {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhubid', usernameVariable: 'docker_user', passwordVariable: 'docker_pass')]) {
                sh "docker tag jenkinstest ${env.docker_user}/jenkinstest:latest"
                sh "docker login -u ${env.docker_user} -p ${env.docker_pass}"
                sh "docker push ${env.docker_user}/jenkinstest:latest"
            }
        }
        }
        stage("Deploy dockerImage") {
          steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhubid', usernameVariable: 'docker_user', passwordVariable: 'docker_pass')]) {
                sh "docker login -u ${env.docker_user} -p ${env.docker_pass}"
                sh "docker run -dit --name web -p 80:80  ${env.docker_user}/jenkinstest:latest"
            }
        }
        }
 
    }

}