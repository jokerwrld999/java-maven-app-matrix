pipeline {
    agent none

    stages {
        stage("Build & Test") {
            matrix {
                agent {
                    dockerfile {
                        label "docker"
                        additionalBuildArgs """
                            --build-arg JAVA_VERSION=$JAVA \
                            --build-arg MAVEN_VERSION=$MAVEN \
                            --build-arg USER_UID=\$(id -u) \
                            -t mymaven:${MAVEN}-jdk-${JAVA}
                        """.stripIndent().trim()

                        args "-v /var/jenkins_home/maven:/home/jenkins/.m2"
                    }
                }
                axes {
                    axis {
                        name "JAVA"
                        values "11.0.24-amzn", "17.0.11-graal", "22.0.1-amzn"
                    }
                    axis {
                        name "MAVEN"
                        values "3.6.3"
                    }
                }
                stages {
                    stage("Build") {
                        steps {
                            sh "mvn -DskipTests clean package"
                        }
                    }
                    stage("Test") {
                        steps {
                            echo "Testing..."
                        }
                    }
                }
            }
        }
    }
}