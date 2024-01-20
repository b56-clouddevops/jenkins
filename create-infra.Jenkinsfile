pipeline {
    agent any 
    parameters {
        choice(name: 'ENV', choices: ['dev', 'prod'], description: 'Select The Environment')
    }
    options {
        ansiColor('xterm')    // Add's color to the output : Ensure you install AnsiColor Plugin.
    }
    stages {
        stage('creating Network') {
            steps {
                git branch: 'main', url: 'https://github.com/b56-clouddevops/terraform-vpc.git'
                        sh "terrafile -f env-${ENV}/Terrafile"
                        sh "terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars  -reconfigure"
                        sh "terraform plan -var-file=env-${ENV}/${ENV}.tfvars"
                        sh "terraform apply -var-file=env-${ENV}/${ENV}.tfvars -auto-approve"
                }
            }

        stage('creating ALB') {
            steps {
                git branch: 'main', url: 'https://github.com/b56-clouddevops/terraform-loadbalancers.git'
                        sh "terrafile -f env-${ENV}/Terrafile"
                        sh "terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure"
                        sh "terraform plan -var-file=env-${ENV}/${ENV}.tfvars"
                        sh "terraform apply -var-file=env-${ENV}/${ENV}.tfvars -auto-approve"
                }
            }

        stage('creating Databases') {
            steps {
                        git branch: 'main', url: 'https://github.com/b56-clouddevops/terraform-databases.git'
                        sh "terrafile -f env-${ENV}/Terrafile"
                        sh "terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure"
                        sh "terraform plan -var-file=env-${ENV}/${ENV}.tfvars"
                        sh "terraform apply -var-file=env-${ENV}/${ENV}.tfvars -auto-approve"
                    }
                }
      
        stage('Backend') {
            parallel {
                stage('creating Catalogue') {
                    steps {
                        dir('catalogue') {   git branch: 'main', url: 'https://github.com/b56-clouddevops/catalogue.git'
                                sh ''' 
                                    cd mutable-infra
                                    terrafile -f env-${ENV}/Terrafile
                                    terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                    terraform plan -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=100
                                    terraform apply -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=100 -auto-approve
                                ''' 
                            }
                        }
                    } 
                stage('creating User') {
                    steps {
                        dir('user') {  git branch: 'main', url: 'https://github.com/b56-clouddevops/user.git'
                                sh ''' 
                                    cd mutable-infra
                                    terrafile -f env-${ENV}/Terrafile
                                    terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                    terraform plan -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=100
                                    terraform apply -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=100 -auto-approve
                                ''' 
                            }
                        }
                    }
                stage('creating Cart') {
                    steps {
                        dir('cart') { git branch: 'main', url: 'https://github.com/b56-clouddevops/cart.git'
                                sh ''' 
                                    cd mutable-infra
                                    terrafile -f env-${ENV}/Terrafile
                                    terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                    terraform plan -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=100
                                    terraform apply -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=100 -auto-approve
                                ''' 
                            }
                        }
                    }
                stage('creating Shipping') {
                    steps {
                        dir('shipping') { git branch: 'main', url: 'https://github.com/b56-clouddevops/shipping.git'
                                sh ''' 
                                    cd mutable-infra
                                    terrafile -f env-${ENV}/Terrafile
                                    terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                    terraform plan -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=100
                                    terraform apply -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=100 -auto-approve
                                ''' 
                            }
                        }
                    }
                stage('creating Payment') {
                    steps {
                        dir('payment') { git branch: 'main', url: 'https://github.com/b56-clouddevops/payment.git'
                                sh ''' 
                                    cd mutable-infra
                                    terrafile -f env-${ENV}/Terrafile
                                    terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                    terraform plan -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=100
                                    terraform apply -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=100 -auto-approve
                                ''' 
                                }
                            }
                        }
                    }
                }
        stage('creating Frontend') {
                steps {
                        dir('frontend') {  git branch: 'main', url: 'https://github.com/b56-clouddevops/frontend.git'
                            sh ''' 
                                cd mutable-infra
                                terrafile -f env-${ENV}/Terrafile
                                terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                terraform plan -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=100
                                terraform apply -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=100 -auto-approve
                            ''' 
                        }
                    }
                }  
            }    
        }                        