#!groovy

node {
    deleteDir()
    def environment = params.env.toLowerCase()
    def connectorName = params.connectorName.toLowerCase()
    def operation = params.operation.toLowerCase()

    def connectorPath = "/connectors/${environment}/${connectorName}.json"

    println "using env=${environment}"

    stage (name: "Clone") {
        checkout scm
        sh "git archive --remote=https://github.com/luizcnn/jenkins-learning --format=tar main ${connectorPath} | tar xf -"
    }
    def connector = readFile "${WORKSPACE}${connectorPath}"
    stage (name: "Execute Operation") {
        if(operation == "create") {
            build(job: './second-pipeline', wait: true, parameters: [
                [$class: 'StringParameterValue', name: 'body', value: connector],
                [$class: 'StringParameterValue', name: 'environment', value: environment]
            ])
        }
    }
}