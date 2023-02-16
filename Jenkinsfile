pipeline {
    agent any
    stages {
        stage("Checkout") {
            steps {
               checkout scm
        }


    }

        stage("update git"){
        steps{
            withCredentials([usernamePassword(credentialsId: 'my-git', passwordVariable: 'pass', usernameVariable: 'user')]) {
                  script {
                        env.encodedPass=URLEncoder.encode(pass, "UTF-8")
                    }

 

        sh 'git config user.email "${GITHUB_EMAIL}"'
        sh 'git config user.name  "${GITHUB_USERNAME}"'
        sh "cat manifest.yaml"
        echo "${DOCKERTAG}"
        sh "sed -i 's+image-registry.openshift-image-registry.svc:5000/amisha-jenkins/expense-tracker-frontend:.*+image-registry.openshift-image-registry.svc:5000/amisha-jenkins/expense-tracker-frontend:${DOCKERTAG}+g' manifest.yaml"
        sh "cat manifest.yaml"
        sh "git add ."
        sh "git commit -m 'done by jenkins frontend-deployment-pipeline' "
       // sh "git pull"
        sh 'git push https://$user:$encodedPass@github.com/$user/frontend-cd.git HEAD:branch'
            }
        }
        }
        
        
         stage("deploy the application") {
        steps {
            script {
                openshift.withCluster() {
                    openshift.withProject("$PROJECT_NAME") {
                        echo "Using project: ${openshift.project()}"
                         sh 'oc project "$PROJECT_NAME"'
                         sh 'oc apply -f manifest.yaml'
                    }
                 }
            }
        } 
    }  
        }
}
