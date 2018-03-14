pipeline {
          agent any

          tools {
                 maven 'MAVEN3.5.3'
          }

          stages {
                  stage ('Build'){
                                  steps {
                                          sh 'mvn clean package'
                                  }

                                  post {
                                        success {
                                        echo 'Now Archiving..'
                                        archiveArtifacts artifacts: '**/target/*.war'
                                        }
                                  }
                  }
                  stage ('Deploy to Staging'){
                                              steps{
                                                    build job: 'Deploy-Staging'
                                              }
                  }
                  stage ('Deploy to production'){
                                                steps{
                                                      timeout(time:5, unit:'Days'){
                                                                                  input message: 'Approve Production Deployment'
                                                      }
                                                      build job: 'Deploy-Production'
                                                }
                                                post {
                                                      success {
                                                              echo 'Deployment was successful'
                                                      }
                                                      failure {
                                                              echo 'Deployment failed'
                                                      }
                                                }
                  }
          }
}
