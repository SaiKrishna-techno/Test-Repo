// pipeline {
//     agent none
//     stages {
	
// 	stage('Non-Parallel Stage') {
// 	    agent {
//                         label "master"
//                 }
//         steps {
//                 echo 'This stage will be executed first'
//                 }
//         }

	
//         stage('Run Tests') {
//             parallel {
//                 stage('Test On Windows') {
//                     agent {
//                         label "Windows_Node"
//                     }
//                     steps {
//                         echo "Task1 on Agent"
//                     }
                    
//                 }
//                 stage('Test On Master') {
//                     agent {
//                         label "master"
//                     }
//                     steps {
// 						echo "Task1 on Master"
// 					}
//                 }
//             }
//         }
//     }
// }

// pipeline {
//     agent any
    
//     stages {
//         stage('Git Checkout') {
//             steps {
//                 script{
//                     try{
//                          echo 'Cloning the Repo'
//                          git branch: 'main', url: 'https://github.com/SaiKrishna-techno/Test-Repo.git'

//                          if (currentBuild.resultIsBetterOrEqualTo("SUCCESS")){
//                              echo "This Build was successfull and this is from try block "
//                             }
//                 // Your build steps here
//                         }
//                     catch (Exception e) {
//                         currentBuild.result = 'FAILURE'
//                         throw e
//                     }
//                     }
//                 }
//             }
//         }
        
//         stage('Build') {
//             steps {
//                 echo 'Bulding'
//                 bat 'Build.bat'
//                 // Your test steps here
//             }
//         }
        
//         stage('Testing') {
//             steps {
//                 echo 'Testing...'
//                 bat 'Unit.bat'
//                 // Your deployment steps here
//             }
//         }
//         stage('QA'){
//             steps{
//                 echo "Quality Analysis"
//                 bat 'Quality.bat'
//             }
//         }
//         stage ("Deploy"){
//             steps{
//                 echo "Deploying"
//                 bat 'Deploy.bat'
//             }
//         }
//     }
    
//     post {
//         always {
//             echo 'This will run always'
//             // Clean up or final actions here
//         }
        
//         success {
//             echo 'This will run on successful build'
//             // Notify or perform actions on success
//         }
        
//         failure {
//             echo 'This will run on build failure'
//             // Perform actions on build failure
//         }
        
//         unstable {
//             echo 'This will run on unstable build'
//             // Perform actions on unstable build
//         }
        
//         changed {
//             echo 'This will run when there are changes'
//             // Perform actions when changes occur
//         }
//     }
// }
// Jenkins file for the pipeline to get the code form git repo,  build it, test it using maven and upon successfull testing will ge pushed to the AWS CodeCommit which will triggger the main pipeline 
// pipeline {
//     agent any
    
//     stages {
//         stage('Checkout') {
//             steps {
//                 checkout scm
//             }
//         }
        
//         stage('Build and Test') {
//             steps {
//                 script {
//                     try {
//                         // Build with Maven
//                         sh 'mvn clean install'
                        
//                         // Run Selenium tests
//                         sh 'mvn test'
                        
//                         // If tests pass, commit to AWS CodeCommit
//                         if (currentBuild.resultIsBetterOrEqualTo("SUCCESS")) {
//                             sh '''
//                                 git config --global user.email "jenkins@example.com"
//                                 git config --global user.name "Jenkins"
//                                 git clone https://git-codecommit.us-east-1.amazonaws.com/v1/repos/my-repo
//                                 cd my-repo
//                                 cp -r ../workspace/* .
//                                 git add .
//                                 git commit -m "Automated commit from Jenkins"
//                                 git push origin master
//                             '''
//                         }
//                     } catch (Exception e) {
//                         currentBuild.result = 'FAILURE'
//                         throw e
//                     }
//                 }
//             }
//         }
//     }
// }
pipeline {
     agent {
         docker{
             image 'myjenkins-blueocean:2.332.3-1'
         }
     }
    // agent any
    parameters{
        choice(name:'VERSION', choices:['1.1.0','1.2.0','1.3.0'], description:'These are the Versions')
        booleanParam(name:'executeTest',defaultValue: true , description:'This variable decides whether the test should run or not ')
    }
    
    stages {
        stage('Git Checkout') {
            steps {
                script {
                    try {
                        echo 'Cloning the Repo'
                        git branch: 'main', url: 'https://github.com/SaiKrishna-techno/Test-Repo.git'

                        if (currentBuild.resultIsBetterOrEqualTo("SUCCESS")) {
                            echo "This Build was successful and this is from try block"
                        }
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        throw e
                    }
                }
            }
        }
        
        stage('Build') {
            steps {
                echo 'Building'
                bat 'Build.bat'
            }
        }
        
        stage('Testing') {
            when{
                expression{
                    params.executeTest
                }
            }
            steps {
                echo 'Testing...'
                bat 'Unit.bat'
            }
        }
        
        stage('QA'){
            steps {
                echo "Quality Analysis"
                bat 'Quality.bat'
            }
        }
        
        stage("Deploy") {
            steps {
                echo "Deploying"
                bat 'Deploy.bat'
            }
        }
    }
    
    post {
        always {
            echo 'This will run always'
            // Clean up or final actions here
        }
        
        success {
            echo 'This will run on successful build'
            // Notify or perform actions on success
        }
        
        failure {
            echo 'This will run on build failure'
            // Perform actions on build failure
        }
        
        unstable {
            echo 'This will run on unstable build'
            // Perform actions on unstable build
        }
        
        changed {
            echo 'This will run when there are changes'
            // Perform actions when changes occur
        }
    }
}
