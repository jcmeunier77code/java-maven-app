pipeline {

    agent any
    tools {
        maven 'maven-3.8.6'
    }

    stages {

            stage("increment version") {

                        steps {
            		        script {
            		        echo "incrementing application version"
            		        sh 'mvn build-helper:parse-version versions:set \
            		        -DnewVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.nextMinorVersion}.\\\${parsedVersion.nextIncrementalVersion} \
            		        versions:commit'
            		        def matcher = readFile('pom.xml') =~ '<version>(.+)</version>'
            		        def version = matcher[0][1]
            		        env.IMAGE_NAME = "$version-$BUILD_NUMBER"
            		        }
                        }
                    }

    stages {

        stage("build jar") {

                    steps {
        		        script {
        		        echo "building the application"
        		        sh 'mvn clean package'
        		        }
                    }
                }

        stage("build image") {

                    steps {
        		        script {
        		        echo "building the docker image"
        		        withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
        		            sh "docker build -t jcmeunier77/bootcamp-java-maven-app:${IMAGE_NAME} ."
        		            sh "echo $PASS | docker login -u $USER --password-stdin"
        		            sh "docker push jcmeunier77/bootcamp-java-maven-app:${IMAGE_NAME}"

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


