pipeline {
    agent{
        label 'bhavana'
    }
    triggers{
        pollSCM('* * * * *')
    }
    stages{
        stage('vcs') {
           steps{
               git url: 'https://github.com/Praneethysp007/bhavapitstop.git',
                   branch: 'main'

        
            }
        }

        stage('build') {
            steps{
                sh(script: 'dotnet build src/pitstop.sln')


                rtUpload(
                    serverId: 'myinstance',
                    spec: """{
                        "files": [
                            {
                                "pattern": "**/*.dll",
                                "target": "pitstop-generic-local/pitstop-generic-local/"
                            }
                        ]
                    }""" 

                )

            }
        }
        // stage('report') {
        //     steps {
        //         junit testResults: '**/*.trx'
        //      }
        // }

        stage('download sonar') {
            steps{

                  sh(script: 'env SONAR_TOKEN=22379b81e94c42a9f1e19710a7fa4422402992f0')
            }
        }
        stage('SonarQube analysis') {
            steps {
                withSonarQubeEnv('sonarcube') {
                          sh 'dotnet build src/pitstop.sln sonar:sonar -Dsonar.organization=myorganisationysp -Dsonar.projectKey=myorganisationysp_pitstop'
                }
            }    
        }



        stage('results') {
            steps{
                archiveArtifacts artifacts : '**/*.dll'
            }
        }    
    }    
        

}