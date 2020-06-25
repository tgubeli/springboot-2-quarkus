#!groovy

node('maven') {

    def mvnCmd = "mvn -s ./nexus_openshift_settings.xml"
    def buidlConfigName = "accountbalance"
    def projectName = "ganck-pg-dev"
    
    stage('Checkout Source') {
        echo '---> Checkout source code ...'
        checkout scm
    }

    def groupId    = getGroupIdFromPom("pom.xml")
    def artifactId = getArtifactIdFromPom("pom.xml")
    def version    = getVersionFromPom("pom.xml")
    def packageName = getGeneratedPackageName(groupId, artifactId, version)
    def jarName = "${artifactId}-${version}.jar"
    
    stage('Build jar') {
        echo '---> Building the sourcec code ...'
        sh "${mvnCmd} package -DskipTests=true"
        //sh "mvn package -DskipTests=true"
        echo "Generated jar file: ${packageName}"
        sh "ls /tmp/workspace/${JOB_NAME}/target/"
    }
    stage('Deploy Apps') {
        echo '---> Deploy apps onto openshift ...'
        sh "mkdir ./deployments"
        sh "cp /tmp/workspace/${JOB_NAME}/target/${jarName} ./deployments/"
        sh "oc start-build ${buidlConfigName} --from-file=./deployments/${jarName} -n ${projectName} --wait=true"
            
    }

}


def getVersionFromPom(pom) {
  def matcher = readFile(pom) =~ '<version>(.+)</version>'
  matcher ? matcher[0][1] : null
}
def getGroupIdFromPom(pom) {
  def matcher = readFile(pom) =~ '<groupId>(.+)</groupId>'
  matcher ? matcher[0][1] : null
}
def getArtifactIdFromPom(pom) {
  def matcher = readFile(pom) =~ '<artifactId>(.+)</artifactId>'
  matcher ? matcher[0][1] : null
}

def getJarName(artifactId){
    "${artifactId}.jar"
}

def getGeneratedPackageName(groupId, artifactId, version){
    String warFileName = "${groupId}.${artifactId}"
    warFileName = warFileName.replace('.', '/')
    "${warFileName}/${version}/${artifactId}-${version}.jar"
}


