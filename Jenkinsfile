pipeline {
    agent any
    stages {
      stage('Code checkout') {
           steps {
               git branch: 'main', url: 'https://github.com/Jeevasanna/project-demo.git'
           }
       } 
        stage('Build artifact') {
            steps {
                 sh 'mvn package'
            }
        }
//         stage('sonar Analysis') {
//             steps{
//                 withSonarQubeEnv('sonarcloud') {
//                    sh 'mvn clean verify sonar:sonar \
//                     -Dsonar.projectName=demo_project \
//                     -Dsonar.projectKey=jeevasanna \
//                     -Dsonar.login=57c5f480bb3dc89755e90b2a4d9dedf2f9bdc4b4 \
//                     -Dsonar.host.url=https://sonarcloud.io \
//                     -Dsonar.organization=jeevasanna'
                    
//                 }
//             }
//         }
        stage('Artifactory-Configuration') {
            steps {
                rtMavenDeployer (
                    id: 'vijay-deployer',
                    serverId: 'JFROG_ARTIFACTORY',
                    releaseRepo: "demo-libs-release",
                    snapshotRepo: "demo-libs-snapshot"
                    
                )
            }
        }
        
        stage('Build & Deploy the Code') {
            steps {
                rtMavenRun (
                    // Tool name from Jenkins configuration.
                    tool: 'MAVEN_BUILD',
                    pom: 'pom.xml',
                    goals: 'install',
                    // Maven options.
                    deployerId: 'vijay-deployer'
                )
            }
        }
        stage('Ansible') {
            steps {
                sh script: "ansible-playbook -i Inventory playbook.yaml"                            
            }
        }	  
    }    
}
