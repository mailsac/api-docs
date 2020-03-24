pipeline {
  agent none
  triggers {
    pollSCM('H/10 * * * *')
  }
  parameters {
    booleanParam(defaultValue: false, description: 'Deploy the build for this commit to production?', name: 'deployProduction')
    string(defaultValue: 'master', description: 'The branch of mailsac-deploy(ansible-playbooks) to use. ', name: 'deployBranch')
  }
  stages {
    stage('Setup') {
      when {
        expression {
          return !params.deployProduction
        }
      }
      agent any
      steps {
        sh 'rm -rf build'
      }
    }
    stage('Build') {
      when {
        expression {
          return !params.deployProduction
        }
      }
      agent {
        docker {
          image 'node:dubnium'
        }
      }
      environment {
        HOME = '.'
      }
      steps {
        echo "commit = ${env.GIT_COMMIT}, user = ${env.USER}"
        sh 'pwd && ls'
        sh 'node -v'
        sh 'npm --version'
        sh 'npm install --loglevel error'
        sh 'npm run build'
      }
    }
    stage('Package') {
      agent any
      when {
        expression {
          return !params.deployProduction
        }
      }
      steps {
        sh 'rm -f api-docs.tgz'
        sh 'tar -C ./build -czf api-docs.tgz .'
        withAWS(credentials:'aws-ansible-key') {
          s3Upload(bucket:'mailsac-jenkins', path:"api-docs/${env.GIT_COMMIT}.tgz", file:'api-docs.tgz')
        }
      }
    }
    stage('Predeploy') {
      agent any
      steps {
        echo 'checking out ansible project'
        checkout changelog: false, poll: false, scm: [$class: 'GitSCM', branches: [[name: "${params.deployBranch}"]], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'CloneOption', noTags: true, reference: '', shallow: true], [$class: 'RelativeTargetDirectory', relativeTargetDir: 'deploy']], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'jenkins-github-upass', url: 'https://github.com/ruffrey/mailsac-deploy.git']]]
      }
    }
    stage('Deploy staging') {
      agent any
      when {
        expression {
          return !params.deployProduction && env.BRANCH_NAME == 'master'
        }
      }
      steps {
        sh "cd deploy; ansible-playbook --user=ubuntu -i inventory/staging.ini -e 'docs_version=${env.GIT_COMMIT}' --limit=frontend api-docs.yml"
      }
    }
    stage('Deploy production') {
      agent any
      when {
        expression {
          return params.deployProduction
        }
      }
      steps {
        sh "cd deploy; ansible-playbook --user=ubuntu -i inventory/aws.ini -e 'docs_version=${env.GIT_COMMIT}' --limit=frontend api-docs.yml"
      }
    }
  }
}

