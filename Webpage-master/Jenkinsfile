pipeline {
    agent any
    environment {
        PATH = "/opt/Maven/apache-maven-3.6.2/bin:$PATH"
        JAVA = "/opt/Java/jdk1.8.0_221/bin:$JAVA"
    }
    
    stages {
        stage ('scm checkout') {
            steps {
                git 'https://github.com/kandepi143/Webpage.git'
            }
        }
        stage ('build') {
            steps {
                sh 'mvn clean install'  
            }
         
        }
        stage ('copy .war to ansible') {
            steps {
//                sh "scp /var/lib/jenkins/workspace/Pipeline/target/subbu.war subbu@10.10.10.200:/home/subbu/war_files/subbu.war"
               sshPublisher(publishers: [sshPublisherDesc(configName: 'Ansible/Docker', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: 'war_files', remoteDirectorySDF: false, removePrefix: 'target', sourceFiles: '**/*.war')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
            }
        }
        stage ('ansible - deploy .war into tomcat') {
            steps {
                sshPublisher(publishers: [sshPublisherDesc(configName: 'Ansible/Docker', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '''cd /home/subbu/playbooks/ ;
ansible-playbook copy.yml ;''', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
            }
        }
    }
}
