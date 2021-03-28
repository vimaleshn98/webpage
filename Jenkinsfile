pipeline{
    agent any
    stages{
        stage("build"){
            steps{
                bat 'mvn -version'
                bat 'mvn compile'
            }
            post{
                success{
                    echo "========Maven compile stage executed successfully========"
                }
                failure{
                    echo "========Maven compile stage execution failed========"
                }
            }
        }
    }
     stage("Test"){
            steps{
                echo "Maven Test"
                bat 'mvn test'
            }
            post{
                success{
                    junit 'target/surefire-reports/**/*.xml'
                    echo "========Maven Test stage executed successfully  ${New_Version}========"

                }
                failure{
                    echo "========Maven Test stage execution failed========"
                }
            }
        }
        stage("Packaging"){
            steps{
                echo "Maven Packaging"
                bat 'mvn package'
            }
            post{
                success{
                    archiveArtifacts 'target/*.jar'
                    echo "========Maven Packaging stage executed successfully  ${New_Version}========"

                }
                
                failure{
                    echo "========Maven Packaging stage execution failed========"
                }
            }
        }
    post{
        always{
            echo "========always========"
        }
        success{
            echo "========pipeline executed successfully ========"
        }
        failure{
            echo "========pipeline execution failed========"
        }
    }
}