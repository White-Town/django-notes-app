
pipeline{
    agent {label "bond"}
    stages{
        stage("Code"){
            steps{
                sh "echo Clonning the code"
                git url : "https://github.com/White-Town/django-notes-app.git", branch: "main"
                echo " code clone successfull"
            }
        }
        stage("Build"){
            steps{
                sh "echo Building the code"
                sh "docker build -t notes-app:latest ."
                echo "Build successfull."
            }
        }
        stage("Push to Dockerhub"){
            steps{
                sh "echo Pushing image to the dockerhub"
                withCredentials([usernamePassword(
                    credentialsId:"docker-hub-cred",
                    passwordVariable:"dockerHubPass",
                    usernameVariable:"dockerHubUser"
                    )]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker image tag notes-app:latest ${env.dockerHubUser}/notes-app:latest"
                sh "docker push ${env.dockerHubUser}/notes-app:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "echo Deploying the code"
                sh "docker compose down && docker compose up -d"
            }
        }
    }
        post{

                always {

                    mail to: 'nareshbc5555@gmail.com',

                    subject: "Job '${JOB_NAME}' (${BUILD_NUMBER}) status",

                    body: "Please go to ${BUILD_URL} and verify the build"

                }
 
                success {

                    mail bcc: '', body: """Hi Team,

                    Build #$BUILD_NUMBER is successful, please go through the url

                    $BUILD_URL

                    and verify the details.

                    Regards,

                    DevOps Team""", cc: '', from: '', replyTo: '', subject: 'BUILD SUCCESS NOTIFICATION', to: 'nareshbc5555@gmail.com'

                }
 
                failure {

                        mail bcc: '', body: """Hi Team,

                        Build #$BUILD_NUMBER is unsuccessful, please go through the url

                        $BUILD_URL

                        and verify the details.

                        Regards,

                        DevOps Team""", cc: '', from: '', replyTo: '', subject: 'BUILD FAILED NOTIFICATION', to: 'nareshbc5555@gmail.com'

                }

         }
}