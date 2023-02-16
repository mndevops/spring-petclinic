pipeline{
    agent any
    stages{
        stage("git clone"){
            steps{
                 git branch: 'main', url: 'https://github.com/virtualmail-rgb/spring-petclinic.git'
            }
        }
        stage ('Artifactory configuration') {
            steps {
                rtServer (
                    id: "JFROG-01-02",
                    url: "https://vardevops123.jfrog.io/",
                    credentialsId: "JFROG_CRED"
                )

                rtMavenDeployer (
                    id: "MAVEN_DEPLOYER",
                    serverId: "JFROG-01-02",
                    releaseRepo: 'qtdevops-libs-release-local',
                    snapshotRepo: 'qtdevops-libs-snapshot-local'
                )
            }
        }
        stage ('Exec Maven') {
            steps {
                rtMavenRun (
                    tool: MVN-DFT, // Tool name from Jenkins configuration
                    pom: 'pom.xml',
                    goals: 'clean install',
                    deployerId: "MAVEN_DEPLOYER"
                )
            }
        }

        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "JFROG-01-02"
                )
            }
        }
    }
}