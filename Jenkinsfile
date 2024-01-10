// PHP + Codeception + Jenkins Pipeline + Allure Report example

node {
    stage('Preparation') { // for display purposes
        // Get some code from a GitHub repository
        // git 'https://github.com/jglick/simple-maven-project-with-tests.git'

        // Or copy project to Jenkins work directory
        sh 'cp -r -f /var/www/html/my_project_name/* ./'
    }
    stage('Build') {
        // Build of project if necessary
    }
    stage('Unit-tests') {
        if ( params.UNIT_TESTS==true ) {
            echo 'Run Unit tests'
        }
    }
    stage('Functional-tests') {
        if ( params.FUNCTIONAL_TESTS==true ) {
            echo 'Run Functional tests'   
        }
    }
    stage('Acceptance-tests') {
        if ( params.ACCEPTANCE_TESTS==true ) {
            echo 'Run Acceptance tests'
            
            // Generate accepance scenarios
            sh 'php vendor/bin/codecept generate:scenarios Acceptance'
            
            // Run acceptance tests with steps
            // sh 'php vendor/bin/codecept run Acceptance --steps --xml --html'
            // or without them
            sh 'php vendor/bin/codecept run Acceptance --xml --html'
        }
    }
    stage('Results') {
    	// Check exist of allure-results folder where will be located *.xml reports files
        if( !fileExists('./allure-results') ) {
            echo 'Dir allure-results is not exist! Creating...'
            sh 'mkdir allure-results'
        }
        
        // Copy *.xml files
        // If new name of file is required
        // def filename = "${currentBuild.startTimeInMillis}"
        // echo filename
        // sh "cp -f ./tests/_output/report.xml ./allure-results/junit-${filename}.xml"
        // or else
        sh 'cp -f ./tests/_output/report.xml ./allure-results/junit.xml'
        
        // Generate Allure-report
        allure includeProperties: false, jdk: 'openjdk 11', results: [[path: '/var/lib/jenkins/workspace/Generate junit.xml/allure-report']]
    }
    stage('Send report to Email') {
        statusText = "${currentBuild.currentResult}"
        if ( statusText=='SUCCESS' ) 
            statusText += ' 🥳'
        else
            statusText += ' ❌'

        mail bcc: '', mimeType: 'text/html', body: "Build status: <b>${statusText}</b><p><a href='${env.BUILD_URL}/allure/'>See details</a>", cc: '', from: '', replyTo: '', subject: "Report Jenkins my_project_name Codeception tests for Build ${env.BUILD_NUMBER} - ${statusText}", to: 'email@mail.com'
    }
}
