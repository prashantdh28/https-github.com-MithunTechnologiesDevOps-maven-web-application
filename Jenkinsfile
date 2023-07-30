pipeline {
    agent any

    stages {
        // stage('Git Clone') {
        //     steps {
        //         git credentialsId: 'aws-cred', url: 'https://github.com/prashantdh28/https-github.com-MithunTechnologiesDevOps-maven-web-application.git'
        //     }
        // }

        stage('Build and Deploy for Dev') {
            when {
                branch 'dev'
            }
            parallel {
                stage('Dev Maven Build') {
                    steps {
                        script {
                            def mavenHome = tool name: "maven", type: "maven"
                            def mavenCMD = "${mavenHome}/bin/mvn"
                            sh "${mavenCMD} clean package"
                        }
                    }
                }
                stage('Dev JFrog Upload') {
                    steps {
                        script {
                            def server = Artifactory.server 'Artifactory'
                            def uploadSpec = """{
                                "files": [
                                    {
                                        "pattern": "target/*.war",
                                        "target": "java-web-app/com.mt/maven-web-application/0.0.1-SNAPSHOT/"
                                    }
                                ]
                            }"""
                            server.upload spec: uploadSpec
                        }
                    }
                }
                stage('Dev Deploy') {
                    steps {
                        sshagent(['dev-tomcat-server']) {
                            sh 'scp -o StrictHostKeyChecking=no target/maven-web-application.war ubuntu@65.0.75.43:/home/ubuntu/tomcat/webapps'
                        }
                    }
                }
            }
        }

        stage('Build and Deploy for Master') {
            when {
                branch 'master'
            }
            parallel {
                stage('Master Maven Build') {
                    steps {
                        script {
                            def mavenHome = tool name: "maven", type: "maven"
                            def mavenCMD = "${mavenHome}/bin/mvn"
                            sh "${mavenCMD} clean package"
                        }
                    }
                }
                stage('Master JFrog Upload') {
                    steps {
                        script {
                            def server = Artifactory.server 'Artifactory'
                            def uploadSpec = """{
                                "files": [
                                    {
                                        "pattern": "target/*.war",
                                        "target": "java-web-app/com.mt/maven-web-application/0.0.1-SNAPSHOT/"
                                    }
                                ]
                            }"""
                            server.upload spec: uploadSpec
                        }
                    }
                }
                stage('Master Deploy') {
                    steps {
                        sshagent(['master-tomcat-server']) {
                            sh 'scp -o StrictHostKeyChecking=no target/maven-web-application.war ubuntu@13.233.119.205:/home/ubuntu/tomcat/webapps'
                        }
                    }
                }
            }
        }

        stage('Build and Deploy for Staging') {
            when {
                branch 'staging'
            }
            parallel {
                stage('Staging Maven Build') {
                    steps {
                        script {
                            def mavenHome = tool name: "maven", type: "maven"
                            def mavenCMD = "${mavenHome}/bin/mvn"
                            sh "${mavenCMD} clean package"
                        }
                    }
                }
                stage('Staging JFrog Upload') {
                    steps {
                        script {
                            def server = Artifactory.server 'Artifactory'
                            def uploadSpec = """{
                                "files": [
                                    {
                                        "pattern": "target/*.war",
                                        "target": "java-web-app/com.mt/maven-web-application/0.0.1-SNAPSHOT/"
                                    }
                                ]
                            }"""
                            server.upload spec: uploadSpec
                        }
                    }
                }
                stage('Staging Deploy') {
                    steps {
                        sshagent(['staging-tomcat-server']) {
                            sh 'scp -o StrictHostKeyChecking=no target/maven-web-application.war ubuntu@65.2.122.57:/home/ubuntu/tomcat/webapps'
                        }
                    }
                }
            }
        }
    }
}
