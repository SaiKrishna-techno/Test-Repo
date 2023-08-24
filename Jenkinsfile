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

pipeline {
    agent any
    
    stages {
        stage('Git Checkout') {
            steps {
                echo 'Cloning the Repo'
                git branch: 'main', url: 'https://github.com/SaiKrishna-techno/Test-Repo.git'
                // Your build steps here
            }
        }
        
        stage('Build') {
            steps {
                echo 'Bulding'
                bat 'Build.bat'
                // Your test steps here
            }
        }
        
        stage('Testing') {
            steps {
                echo 'Testing...'
                bat 'Unit.bat'
                // Your deployment steps here
            }
        }
        stage('QA'){
            steps{
                echo "Quality Analysis"
                bat 'Quality.bat'
            }
        }
        stage ("Deploy"){
            steps{
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