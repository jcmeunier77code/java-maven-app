pipeline {

    agent any
    tools {
        maven 'maven-3.8.6'
    }

    stages {

        stage("build jar") {

                    steps {
        		        script {
        		        echo "building the application"
        		        sh 'mvn package'
        		        }
                    }
                }

        stage("build image") {

                    steps {
        		        script {
        		        echo "building the docker image"
        		        withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
        		            sh 'docker build -t jcmeunier77/bootcamp-java-maven-app:jma-3.0 .'
        		            sh "echo $PASS | docker login -u $USER --password-stdin"
        		            sh "docker push jcmeunier77/bootcamp-java-maven-app:jma-3.0"

        		        }
        		        }
        		    }
                 }



        stage("deploy") {
                    steps {
                        script {
        		        echo "deploying the application..."
        		        echo "c'est la fête du slip"
                        }
                    }
                }
        }
    }


