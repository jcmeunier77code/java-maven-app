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
        stage("commit version update") {
            steps {
                script {
                    withCredentials([sshUserPrivateKey(credentialsId: "da38a375-03b3-4b5f-91af-f50d2a0665b9", keyFileVariable: 'keyfile')]) {
                        sh 'git config user.email "jenkins@example.com"'
                        sh 'git config user.name "jenkins"'

                        sh "cat $keyfile"
                        sh "echo $keyfile > ~/.ssh/id_rsa.pub"
                        sh 'git status'
                        sh 'git branch'
                        sh 'git config --list'

                        sh 'git remote set-url origin git@github.com:jcmeunier77code/java-maven-app.git'
                        sh 'git add .'
                        sh 'git commit -m "ci: version bump"'
                        sh 'git push origin HEAD:jenkins-jobs'

                    }
                }
            }
        }
    }
}


