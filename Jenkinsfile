pipeline {
    agent any
    environment {
        NEXUS_USER         = credentials('NEXUS-USER')
        NEXUS_PASSWORD     = credentials('NEXUS-PASS')
    }
    parameters {
        choice(
            name:'compileTool',
            choices: ['Maven', 'Gradle'],
            description: 'Seleccione herramienta de compilacion'
        )
    }
    stages {
        stage("Pipeline"){
            steps {
                script{
                  switch(params.compileTool)
                    {
                        case 'Maven':
                            def ejecucion = load 'maven.groovy'
                            ejecucion.call()
                        break;
                        case 'Gradle':
                            def ejecucion = load 'gradle.groovy'
                            ejecucion.call()
                        break;
                    }
                }
            }
            post{
                success{
                    //slackSend color: 'good', message: "Ernesto [${JOB_NAME}] [${BUILD_TAG}] Ejecucion Exitosa. Ejecutado con [${params.compileTool}]", teamDomain: 'dipdevopsusac-tr94431', tokenCredentialId: 'slack-token'
                    slackSend color: 'good', message: "[Mentor] [${JOB_NAME}] [${BUILD_TAG}] Ejecucion Exitosa", teamDomain: 'dipdevopsusac-tr94431'

                }
                failure{
                    slackSend color: 'danger', message: "[Mentor] [${env.JOB_NAME}] [${BUILD_TAG}] Ejecucion fallida en stage [${env.TAREA}]", teamDomain: 'dipdevopsusac-tr94431'
                    //slackSend color: 'danger', message: "Ernesto [${env.JOB_NAME}] [${BUILD_TAG}] Ejecucion fallida en stage [${env.TAREA}]. Ejecutado con [${params.compileTool}]", teamDomain: 'dipdevopsusac-tr94431', tokenCredentialId: 'slack-token'
                }
            }
        }
    }
}