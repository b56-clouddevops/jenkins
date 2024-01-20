pipeline {
    agent any 
    parameters {
        choice(name: 'ENV', choices: ['dev', 'prod'], description: 'Select The Environment')
    }
    options {
        ansiColor('xterm')    // Add's color to the output : Ensure you install AnsiColor Plugin.
    }
    stages {
        stage('Creating Network') {
            steps {
                git branch: 'main', url: 'https://github.com/b56-clouddevops/terraform-vpc.git'
                        sh "rm -rf .terraform"
                        sh "terrafile -f env-${ENV}/Terrafile"
                        sh "terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars  -reconfigure"
                        sh "terraform plan -var-file=env-${ENV}/${ENV}.tfvars"
                        sh "terraform apply -var-file=env-${ENV}/${ENV}.tfvars -auto-approve"
                }
            }

        stage('Creating ALB') {
            steps {
                   git branch: 'main', url: 'https://github.com/b56-clouddevops/terraform-loadbalancers.git'
                        sh "rm -rf .terraform"
                        sh "terrafile -f env-${ENV}/Terrafile"
                        sh "terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars  -reconfigure"
                        sh "terraform plan -var-file=env-${ENV}/${ENV}.tfvars"
                        sh "terraform apply -var-file=env-${ENV}/${ENV}.tfvars -auto-approve"
                   }
                }

        stage('Creating Databases') {
            steps {
                   git branch: 'main', url: 'https://github.com/b56-clouddevops/terraform-databases.git'
                        sh "rm -rf .terraform"
                        sh "terrafile -f env-${ENV}/Terrafile"

                        sh "terraform init -backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure"
                        sh "terraform plan -var-file=env-${ENV}/${ENV}.tfvars"
                        sh "terraform apply -var-file=env-${ENV}/${ENV}.tfvars -auto-approve"
                   }
                }
        stage('Backend') {
            parallel {
                stage('Creating Catalogue') {
                    steps {
                        dir('catalogue') {   git branch: 'main', url: 'https://github.com/b56-clouddevops/catalogue.git'
                                sh ''' 
                                    cd mutable-infra
                                    rm -rf .terraform
                                    terrafile -f env-${ENV}/Terrafile
                                    terraform init -backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                    terraform plan -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=0.0.6
                                    terraform apply -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=0.0.6 -auto-approve

                                ''' 
                            }
                        }
                    } 
                stage('Creating User') {
                    steps {
                        dir('user') {  git branch: 'main', url: 'https://github.com/b56-clouddevops/user.git'
                                sh ''' 
                                    cd mutable-infra
                                    rm -rf .terraform
                                    terrafile -f env-${ENV}/Terrafile
                                    terraform init -backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                    terraform plan -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=0.0.6
                                    terraform apply -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=0.0.6 -auto-approve
                                ''' 
                            }
                        }
                    }
                stage('Creating Cart') {
                    steps {
                        dir('cart') { git branch: 'main', url: 'https://github.com/b56-clouddevops/cart.git'
                                sh ''' 
                                    cd mutable-infra
                                    rm -rf .terraform
                                    terrafile -f env-${ENV}/Terrafile
                                    terraform init -backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                    terraform plan -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=0.0.6
                                    terraform apply -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=0.0.6 -auto-approve
                                ''' 
                            }
                        }
                    }
                stage('Creating Shipping') {
                    steps {
                        dir('shipping') { git branch: 'main', url: 'https://github.com/b56-clouddevops/shipping.git'
                                sh ''' 
                                    cd mutable-infra
                                    rm -rf .terraform
                                    terrafile -f env-${ENV}/Terrafile
                                    terraform init -backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                    terraform plan -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=0.0.6
                                    terraform apply -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=0.0.6 -auto-approve
                                ''' 
                            }
                        }
                    }
                stage('Creating Payment') {
                    steps {
                        dir('payment') { git branch: 'main', url: 'https://github.com/b56-clouddevops/payment.git'
                                sh ''' 
                                    cd mutable-infra
                                    rm -rf .terraform
                                    terrafile -f env-${ENV}/Terrafile
                                    terraform init -backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                    terraform plan -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=0.0.6
                                    terraform apply -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=0.0.6 -auto-approve
                                ''' 
                                }
                            }
                        }
                    }
                }
        stage('Creating Frontend') {
                    steps {
                        dir('frontend') { git branch: 'main', url: 'https://github.com/b56-clouddevops/frontend.git'
                                sh ''' 
                                    cd mutable-infra
                                    rm -rf .terraform
                                    terrafile -f env-${ENV}/Terrafile
                                    terraform init -backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                    terraform plan -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=0.0.6
                                    terraform apply -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=0.0.6 -auto-approve
                                ''' 
                        }
                    }
                }
            }                
        }    
                   