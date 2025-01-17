#!/usr/bin/env groovy

def server = Artifactory.server 'sixriver'
parallel(
    failFast: true,
     "amd64-bionic": {
        node('docker && amd64') {
          def customImage = ""
          def scmVars = ""
          try {
            stage("Build Docker Image") {
                scmVars = checkout scm
                sh 'sed -e "s/#DOCKER_IMAGE/ros:melodic/g" 6river.dockerfile > 6river-melodic-amd64.dockerfile'
                customImage = docker.build("gcr.io/plasma-column-128721/abseil-builder-melodic:amd64", " --file 6river-melodic-amd64.dockerfile ." )
            }
            stage("Build and Publish") {
              customImage.inside("-u 0:0 -e GIT_BRANCH=${scmVars.GIT_BRANCH}") {
                withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'artifactory_apt',
                        usernameVariable: 'ARTIFACTORY_USERNAME', passwordVariable: 'ARTIFACTORY_PASSWORD']]) {
                    withCredentials([string(credentialsId: 'github-access-token', variable: 'GITHUB_TOKEN')]) {
                      sh '''
                        export ARCHITECTURE=amd64
                        export DISTRO=bionic
                        export ROS_DISTRO=melodic
                        ./install.sh
                      '''
                    }
                  }
                }

            }
          } catch (e) {
          echo 'This will run only if failed'
            // Since we're catching the exception in order to report on it,
            // we need to re-throw it, to ensure that the build is marked as failed
            throw e
          } finally {
            echo "Running finally statement"
            stage("Cleanup") {
              customImage.inside('-u 0:0') {
                sh "chmod -R 777 ."
              }
            }
          }
        }
    },
    "amd64-xenial": {
        node('docker && amd64') {
          def customImage = ""
          def scmVars = ""
          try {
            stage("Build Docker Image") {
                scmVars = checkout scm
                sh 'sed -e "s/#DOCKER_IMAGE/ros:kinetic/g" 6river.dockerfile > 6river-kinetic-amd64.dockerfile'
                customImage = docker.build("gcr.io/plasma-column-128721/abseil-builder-kinetic:amd64", " --file 6river-kinetic-amd64.dockerfile ." )
            }
            stage("Build and Publish") {
              customImage.inside("-u 0:0 -e GIT_BRANCH=${scmVars.GIT_BRANCH}") {
                withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'artifactory_apt',
                        usernameVariable: 'ARTIFACTORY_USERNAME', passwordVariable: 'ARTIFACTORY_PASSWORD']]) {
                    withCredentials([string(credentialsId: 'github-access-token', variable: 'GITHUB_TOKEN')]) {
                      sh '''
                        export ARCHITECTURE=amd64
                        export DISTRO=xenial
                        export ROS_DISTRO=kinetic
                        ./install.sh
                      '''
                    }
                  }
                }

            }
          } catch (e) {
          echo 'This will run only if failed'
            // Since we're catching the exception in order to report on it,
            // we need to re-throw it, to ensure that the build is marked as failed
            throw e
          } finally {
            echo "Running finally statement"
            stage("Cleanup") {
              customImage.inside('-u 0:0') {
                sh "chmod -R 777 ."
              }
            }
          }
        }
    },
    "arm64-bionic": {
        node('docker && arm64') {
          def customImage = ""
          def scmVars = ""
          try {
            stage("Build Docker Image") {
                scmVars = checkout scm
                sh 'sed -e "s/#DOCKER_IMAGE/arm64v8\\/ros:melodic/g" 6river.dockerfile > 6river-melodic-arm64.dockerfile'
                customImage = docker.build("gcr.io/plasma-column-128721/abseil-builder-melodic:arm64", " --file 6river-melodic-arm64.dockerfile ." )
            }
            stage("Build and Publish") {
              customImage.inside("-u 0:0 -e GIT_BRANCH=${scmVars.GIT_BRANCH}") {
                withCredentials([string(credentialsId: 'github-access-token', variable: 'GITHUB_TOKEN')]) {
                  withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'artifactory_apt',
                          usernameVariable: 'ARTIFACTORY_USERNAME', passwordVariable: 'ARTIFACTORY_PASSWORD']]) {
                      sh '''
                      export ARCHITECTURE=arm64
                      export DISTRO=bionic
                      export ROS_DISTRO=melodic
                      ./install.sh
                      '''
                  }
                }
              }
            }
          } catch (e) {
            echo 'This will run only if failed'
            // Since we're catching the exception in order to report on it,
            // we need to re-throw it, to ensure that the build is marked as failed
            throw e
          } finally {
            echo "Running arm64 finally statement"
            stage("Cleanup") {
              echo "Inside cleanup stage"
              customImage.inside('-u 0:0') {
                sh "chmod -R 777 ."
              }
            }
          }
        }
    },
    "arm64-xenial": {
        node('docker && arm64') {
          def customImage = ""
          def scmVars = ""
          try {
            stage("Build Docker Image") {
                scmVars = checkout scm
                sh 'sed -e "s/#DOCKER_IMAGE/arm64v8\\/ros:kinetic/g" 6river.dockerfile > 6river-kinetic-arm64.dockerfile'
                customImage = docker.build("gcr.io/plasma-column-128721/abseil-builder-kinetic:arm64", " --file 6river-kinetic-arm64.dockerfile ." )
            }
            stage("Build and Publish") {
              customImage.inside("-u 0:0 -e GIT_BRANCH=${scmVars.GIT_BRANCH}") {
                withCredentials([string(credentialsId: 'github-access-token', variable: 'GITHUB_TOKEN')]) {
                  withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'artifactory_apt',
                          usernameVariable: 'ARTIFACTORY_USERNAME', passwordVariable: 'ARTIFACTORY_PASSWORD']]) {
                      sh '''
                      export ARCHITECTURE=arm64
                      export DISTRO=xenial
                      export ROS_DISTRO=kinetic
                      ./install.sh
                      '''
                  }
                }
              }
            }
          } catch (e) {
            echo 'This will run only if failed'
            // Since we're catching the exception in order to report on it,
            // we need to re-throw it, to ensure that the build is marked as failed
            throw e
          } finally {
            echo "Running arm64 finally statement"
            stage("Cleanup") {
              echo "Inside cleanup stage"
              customImage.inside('-u 0:0') {
                sh "chmod -R 777 ."
              }
            }
          }
        }
    },
)