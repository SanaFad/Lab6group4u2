def commit_id
pipeline{
        agent any
        stages {
            stage('preparation'){
                steps {
                    checkout scm
                    sh "git rev-parse --short HEAD > .git/commit-id"
                    script {
                        commit_id = readFile ('.git/commit-id').trim()
                    }
                }
            }
            stage('build'){
                steps {
                    echo '1. Building Maven workload'
                    sh "mvn clean install"
                    echo "Build complete"
                }
            }

            stage('image build'){
                steps {
                    echo '2. Building Docker  image'
                    sh "docker build -t Lab6group4u2/position-simulator:${commit_id} ."
                    echo 'Docker image built'
                }
            }
            stage("image push"){                                                                                                                                                           steps {                                                                                                                                                                         echo '3. Pushing Docker  Image'                                                                                                                                            sh "docker push Lab6group4u2/position-simulator:${commit_id} "                                                                                                              echo 'Docker image pushed'                                                                                                                                               }                                                                                                                                                                                      }    

            stage('deploy'){
                steps {
                     echo '4.Deployemnt To Kubernetes'
                    sh 'sed -i -r 's|richardchesterwood/k8s-fleetman-webapp-angular|Lab6group4u2/position-simulator:${commit_id}|' workloads.yaml"
                    sh "kubectl apply -f ./workloads.yaml"
                    echo "Deployment completed"
                }
            }


        }


}
