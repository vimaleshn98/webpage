pipeline{
    environment { 
        registry = "7349216534/webapp" 
        registryCredential = 'dockerhub_id' 
        dockerImage = '' 
    }
    agent any
    stages{
        stage("build"){
            steps{
                bat 'mvn clean'
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
    
     stage("Test"){
            steps{
                echo "Maven Test"
                bat 'mvn -Dmaven.test.failure.ignore=true test'
            }
            post{
                success{
                    junit 'target/surefire-reports/**/*.xml'
                    echo "========Maven Test stage executed successfully========"

                }
                failure{
                    echo "========Maven Test stage execution failed========"
                }
            }
        }
        stage("Packaging"){
            steps{
                echo "Maven Packaging"
                bat 'mvn -B -DskipTests clean package'
            }
            post{
                success{
                    archiveArtifacts 'target/*.jar'
                    echo "========Maven Packaging stage executed successfully========"

                }
                
                failure{
                    echo "========Maven Packaging stage execution failed========"
                }
            }
        }
        stage('Building our image') { 
            steps { 
                script { 
                    dockerImage = docker.build registry + ":$BUILD_NUMBER" 
                }
            } 
        }
        stage('Deploy our image') { 
            steps { 
                script {              
                    docker.withRegistry( '', registryCredential ) { 
                        dockerImage.push() 
                    }
                } 
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