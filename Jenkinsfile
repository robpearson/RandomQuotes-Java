pipeline {
    //  parameters here provide the shared values used with each of the Octopus pipeline steps.
    parameters {
        // The space ID that we will be working with. The default space is typically Spaces-1.
        string(defaultValue: 'Spaces-1', description: '', name: 'SpaceId', trim: true)
        // The Octopus project we will be deploying.
        string(defaultValue: 'RandomQuotes', description: '', name: 'ProjectName', trim: true)
        // The environment we will be deploying to.
        string(defaultValue: 'Dev', description: '', name: 'EnvironmentName', trim: true)
        // The name of the Octopus instance in Jenkins that we will be working with. This is set in:
        // Manage Jenkins -> Configure System -> Octopus Deploy Plugin
        string(defaultValue: 'Octopus', description: '', name: 'ServerId', trim: true)
    }
    /*
        These are the tools we need for this pipeline. They are defined in Manage Jenkins -> Global Tools Configuration.
    */
    tools {
        maven 'Maven 3'
        jdk 'Java'
    }
    agent any
    stages {
        /*
            The OctoCLI tool has been defined with the Custom Tools plugin: https://plugins.jenkins.io/custom-tools-plugin/
            This is a convenient way to have a tool placed on an agent, especially when using the Jenkins Docker image.
            This plugin will extract a .tar.gz file (for example https://download.octopusdeploy.com/octopus-tools/7.3.7/OctopusTools.7.3.7.linux-x64.tar.gz)
            to a directory like /var/jenkins_home/tools/com.cloudbees.jenkins.plugins.customtools.CustomTool/OctoCLI/Octo.
            This directory is then specified as the default location of the Octo CLI in Jenkins under
            Manage Jenkins -> Global Tools Configuration -> Octopus Deploy CLI.
        */
        stage ('Add tools') {
            steps {
                sh "echo \"OctoCLI: ${tool('OctoCLI')}\""
            }
        }
        stage('build') {
            steps {
                // Update the Maven project version to match the current build
                sh(script: "mvn versions:set -DnewVersion=1.0.${BUILD_NUMBER}", returnStdout: true)
                // Package the code
                sh(script: "mvn package", returnStdout: true)
            }
        }
    }
}
