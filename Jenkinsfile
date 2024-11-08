pipeline {
    agent { label 'rajesh-laptop' }
    environment {
        COMMIT_AUTHOR = ''
        FAILURE_REASON = ''
    }
    stages {
        stage('Clone Repository') {
            when { branch 'main' }
            steps {
                checkout scm
            }
        }
        stage('Copy Files') {
            when { branch 'main' }
            steps {
                script {
                    sh "cp -r * /docker-data/docker-jenkins-2"
                }
            }
        }
        stage('Build Docker Image') {
            when { branch 'main' }
            steps {
                script {
                    sh 'docker build -t ocs:2.12.1 /docker-data/docker-jenkins-2'
                }
            }
        }
        stage('Run Docker Container') {
            when { branch 'main' }
            steps {
                script {
                    sh 'docker run -d --name ocs-server -p 5000:5000 ocs:2.12.1'
                }
            }
        }
    }
}

    //post {
    //    success {
    //        script {
    //            if (env.BRANCH_NAME == 'main') {
    //                sendEmail('SUCCESS')
    //            }
    //        }
    //    }
    //    failure {
    //        script {
    //            if (env.BRANCH_NAME == 'main') {
    //                FAILURE_REASON = currentBuild.rawBuild.getLog(50).join('\n')
    //                sendEmail('FAILURE', FAILURE_REASON)
    //            }
    //        }
    //    }
    //    always {
    //        script {
    //            if (env.BRANCH_NAME == 'main') {
    //                COMMIT_AUTHOR = sh(script: "git log -1 --pretty=%an", returnStdout: true).trim()
    //            }
    //        }
    //    }
    //}
//}

//def sendEmail(String status, String failureReason = '') {
//    def buildDuration = currentBuild.durationString ?: 'N/A'
//    def buildChangesCount = currentBuild.changeSets.collect { it.items.size() }.sum() ?: 0
//    def commitMessages = []
//    def changedFiles = []
//    
//    currentBuild.changeSets.each { changeSet ->
//        changeSet.items.each { change ->
//            commitMessages.add("${change.commitId} - ${change.msg}")
//            change.affectedFiles.each { file ->
//                changedFiles.add(file.path)
//            }
//        }
//    }
//    
//    def commitMessagesHtml = commitMessages ? commitMessages.join('<br/>') : 'No changes detected.'
//    def changedFilesHtml = changedFiles ? changedFiles.join('<br/>') : 'No files changed.'
//    def failureReasonHtml = status == 'FAILURE' && failureReason ? 
//        "<p><strong>Failure Reason:</strong><br/>${failureReason.replaceAll('&', '&amp;').replaceAll('<', '&lt;').replaceAll('>', '&gt;').replaceAll('\n', '<br/>')}</p>" : ''
//
//    mail to: 'rajesh@atharvasystem.com',
//         subject: "[${env.JOB_NAME}] Jenkins Build Notification - ${status}",
//         body: """<!DOCTYPE html>
//<html>
//<head>
//    <style>
//        body { font-family: Arial, sans-serif; color: #333; background-color: #F4F4F9; }
//        .container { max-width: 600px; margin: 0 auto; padding: 20px; border: 1px solid #ddd; border-radius: 8px; background-color: #FFFFFF; }
//        .header { background-color: ${status == 'SUCCESS' ? '#4CAF50' : '#D9534F'}; color: #FFFFFF; padding: 15px; text-align: center; border-radius: 8px 8px 0 0; }
//        .header h1 { margin: 0; font-size: 24px; }
//        .content { padding: 20px; }
//        .section { margin-top: 15px; padding: 10px; border: 1px solid #ddd; border-radius: 5px; background-color: #F9F9F9; }
//        .section p { margin: 0; font-size: 14px; }
//        .footer { text-align: center; margin-top: 20px; font-size: 12px; color: #777; }
//        a.button { display: inline-block; padding: 10px 20px; color: #FFFFFF; background-color: ${status == 'SUCCESS' ? '#4CAF50' : '#D9534F'}; text-decoration: none; border-radius: 5px; margin-top: 20px; }
//    </style>
//</head>
//<body>
//    <div class="container">
//        <div class="header">
//            <h1>Jenkins Build Notification - ${status}</h1>
//        </div>
//        <div class="content">
//            <p>Dear ${COMMIT_AUTHOR},</p>
//            <p>Your Jenkins build has completed with status: <strong>${status}</strong>. Here are the details:</p>
//            <div class="section">
//                <p><strong>Build Status:</strong> ${status}</p>
//                <p><strong>Build Duration:</strong> ${buildDuration}</p>
//                <p><strong>Number of Changes:</strong> ${buildChangesCount}</p>
//            </div>
//            ${failureReasonHtml}
//            <div class="section">
//                <p><strong>Commit Messages:</strong><br/> ${commitMessagesHtml}</p>
//            </div>
//            <div class="section">
//                <p><strong>Changed Files:</strong><br/> ${changedFilesHtml}</p>
//            </div>
//            <p>For more details, please visit the Jenkins job page:</p>
//            <a href="${env.BUILD_URL}" class="button">View Job</a>
//        </div>
//        <div class="footer">
//            <p>Sent by Jenkins CI/CD Pipeline</p>
//        </div>
//    </div>
//</body>
//</html>""",
//    mimeType: 'text/html'
//}
//