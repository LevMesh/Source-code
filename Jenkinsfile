pipeline {
  agent any

//   parameters {
//     string defaultValue: '0', description: 'Release Version', name: 'version'
//   }

    stages {
        stage('STAGE 1 check if branch exists') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'main') {
                        echo '---------Hello from main branch--------------'
                    
                        script {
                    
                            sh "git config --global user.email 'levmeshorer16@gmail.com'"
                            sh "git config --global user.name 'Lev Meshorer (EC2 JENKINS)'"
                            sh "git fetch --tags"
                            majorMinor = sh(script: "git tag -l --sort=v:refname | tail -1 | cut -c 1-3", returnStdout: true).trim()
                            echo "$majorMinor"
                            previousTag = sh(script: "git describe --tags --abbrev=0 | grep -E '^$majorMinor' || true", returnStdout: true).trim()  // x.y.z or empty string. grep is used to prevent returning a tag from another release branch; true is used to not fail the pipeline if grep returns nothing.
                            echo "$previousTag"
                            if (!previousTag) {
                            patch = "0"
                            } else {
                            patch = (previousTag.tokenize(".")[2].toInteger() + 1).toString()
                            }
                            env.VERSION = majorMinor + "." + patch
                            echo env.version
                            echo env.BRANCH_NAME
                                // cd java-maven-app/

                                // sh "docker build -t tomer:$env.version ." 

                                // sh "git checkout remotes/origin/release/${version}"
                                // sh "git checkout release/${version}"
                                // sh "git pull origin release/${version}"
                                // echo '~~~~~~~~~ BRANCH EXISTS - checkout & pull ~~~~~~~~~~'
                            // } catch (Exception e) {
                            //     sh 'git checkout main'
                            //     sh "git checkout -b release/${version}"
                            //     sh "echo ${version} > v.txt"
                            //     sh "echo 'NOT FOR RELEASE' >> v.txt"
                            //     sh "git commit -am 'Automated commit ${version}'"
                            //     sh "git push origin release/${version}"
                            //     echo '~~~~~~~~~ BRANCH NOT EXISTS - created branch & pushed ~~~~~~~~~'
                            // }
                            
                        }
                    }
                }
            }
        }

        stage ('Stage 2 - build the docker image') {
            steps {
                script {

                    sh "cd java-maven-app/"
                    sh "ls java-maven-app/"
                    sh 'pwd'
                    sh 'ls'
                    sh "docker build -t levvv/java-maven-app:$env.version java-maven-app/"
                    withCredentials([[$class: "UsernamePasswordMultiBinding", credentialsId: "28010381-59f2-49ac-a34a-b5d02d354162", usernameVariable: "G_USER", passwordVariable: "G_PASS"]]) {
                        sh "docker login --username=$G_USER --password=$G_PASS"

                        sh "docker push levvv/java-maven-app:$env.version"
                    }
                }
            }
        }







    // stage ('STAGE 2 find latest tag') {
    //   steps {
    //     sh 'git fetch --tags'

    //     script {
    //       try {
    //         // new_tag = Integer.parseInt( sh(script: "git tag | grep ${version} | sort --version-sort | tail -1 | cut -d '.' -f 3", returnStdout: true).trim() )
    //         new_tag = Integer.parseInt(sh(script: "git tag | grep ${version} | sort --version-sort | tail -1 | cut -d '.' -f 3", returnStdout: true).trim())
    //         echo "${new_tag}"
    //         new_tag += 1
    //       } catch (Exception e) {
    //         new_tag = 0
    //       }
    //       new_version = "${version}.${new_tag}"
    //     }
    //   }
    // }

    // stage ('STAGE 3 build & run') {
    //   steps {
    //     sh "docker build -t ron_cowsay:${new_version} ."
    //     sh "docker run --name app --network project_lab_net -p 4001:8080 -d ron_cowsay:${new_version}"
    //   }
    // }

    // stage ('STAGE 4 Tests') {
    //   steps {
    //     sh 'wget --tries=10 --waitretry=5 --retry-connrefused -O- app:8080'
    //   }
    // }

    // stage ('STAGE 5 Release (push to ECR)') {

    // }

    }

//   post {
//     always {
//       //sh 'docker rm -f app'
//       cleanWs()
//     }
//   }
}