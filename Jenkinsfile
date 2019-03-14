
jettyUrl = 'http://localhost:8081/'

pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Run Flyway Github'
                git 'https://github.com/aboussetta/flyway.git'
		checkout scm
                stash includes: '*.sql', name: 'db' 
		sh 'cd /Users/abderrahim.boussetta/.jenkins/workspace/flyway_pipeline_oracle'
            }
        }
        stage('Build - DB Migration') {
            environment {
		FLYWAY_LOCATIONS='filesystem:/Users/abderrahim.boussetta/.jenkins/workspace/flyway_pipeline_oracle/flyway'
                FLYWAY_URL='jdbc:oracle:thin:@//hhdora-scan.dev.hh.perform.local:1521/DV_FLYWAY'
                FLYWAY_USER='flyway'
                FLYWAY_PASSWORD='flyway_123'
                FLYWAY_SCHEMAS='FLYWAY'
            }
            steps {
                echo 'Run Flyway Migration'
		unstash 'db'
                sh '/Users/abderrahim.boussetta/.jenkins/tools/sp.sd.flywayrunner.installation.FlywayInstallation/flyway_420/flyway -user=$FLYWAY_USER -password=$FLYWAY_PASSWORD -url=$FLYWAY_URL -locations=$FLYWAY_LOCATIONS migrate'
	    	}
        }
	milestone 1
        stage('Parallel - Dev Deployment') {
            failFast true // first to fail abort parallel execution
            parallel {
		stage('DEVA - DB Deployment') {
		          environment {
				FLYWAY_LOCATIONS='filesystem:/Users/abderrahim.boussetta/.jenkins/workspace/flyway_pipeline_oracle/flyway'
		                FLYWAY_URL='jdbc:oracle:thin:@//hhdora-scan.dev.hh.perform.local:1521/DVA_FLYWAY'
		                FLYWAY_USER='flyway_deva'
		                FLYWAY_PASSWORD='flyway_123'
		                FLYWAY_SCHEMAS='FLYWAY_DEVA'
		            }
		            steps {
		                echo 'Run Flyway Migration'
				unstash 'db'
		                sh '/Users/abderrahim.boussetta/.jenkins/tools/sp.sd.flywayrunner.installation.FlywayInstallation/flyway_420/flyway -user=$FLYWAY_USER -password=$FLYWAY_PASSWORD -url=$FLYWAY_URL -locations=$FLYWAY_LOCATIONS migrate'
				input message: 'Continue?'
			    }
		 }
		 stage('DEVB - DB Deployment') {
		            environment {
				FLYWAY_LOCATIONS='filesystem:/Users/abderrahim.boussetta/.jenkins/workspace/flyway_pipeline_oracle/flyway'
		                FLYWAY_URL='jdbc:oracle:thin:@//hhdora-scan.dev.hh.perform.local:1521/DVB_FLYWAY'
		                FLYWAY_USER='flyway_devb'
		                FLYWAY_PASSWORD='flyway_123'
		                FLYWAY_SCHEMAS='FLYWAY_DEVB'
		            }
		            steps {
		                echo 'Run Flyway Migration'
				unstash 'db'
		                sh '/Users/abderrahim.boussetta/.jenkins/tools/sp.sd.flywayrunner.installation.FlywayInstallation/flyway_420/flyway -user=$FLYWAY_USER -password=$FLYWAY_PASSWORD -url=$FLYWAY_URL -locations=$FLYWAY_LOCATIONS migrate'            
			    	input message: 'Continue?'	
			    }
		        }
		    }
		}
	    milestone 2
	    stage('Parallel - Stage Deployment') {
            	failFast true // first to fail abort parallel execution
            	parallel {
			stage('STA - DB Deployment') {
		            environment {
					FLYWAY_LOCATIONS='filesystem:/Users/abderrahim.boussetta/.jenkins/workspace/flyway_pipeline_oracle/flyway'
					FLYWAY_URL='jdbc:oracle:thin:@//hhdora-scan.dev.hh.perform.local:1521/STA_FLYWAY'
					FLYWAY_USER='flyway_sta'
					FLYWAY_PASSWORD='flyway_123'
					FLYWAY_SCHEMAS='FLYWAY_STA'
		            }
		            steps {
		                echo 'Run Flyway Migration'
				unstash 'db'
		                sh '/Users/abderrahim.boussetta/.jenkins/tools/sp.sd.flywayrunner.installation.FlywayInstallation/flyway_420/flyway -user=$FLYWAY_USER -password=$FLYWAY_PASSWORD -url=$FLYWAY_URL -locations=$FLYWAY_LOCATIONS migrate'            
			    	input message: 'Continue?'
			    }
		        }
		        stage('STB - DB Deployment') {
		            environment {
				FLYWAY_LOCATIONS='filesystem:/Users/abderrahim.boussetta/.jenkins/workspace/flyway_pipeline_oracle/flyway'
		                FLYWAY_URL='jdbc:oracle:thin:@//hhdora-scan.dev.hh.perform.local:1521/STB_FLYWAY'
		                FLYWAY_USER='flyway_stb'
		                FLYWAY_PASSWORD='flyway_123'
		                FLYWAY_SCHEMAS='FLYWAY_STB'
		            }
		            steps {
		                echo 'Run Flyway Migration'
				unstash 'db'
		                sh '/Users/abderrahim.boussetta/.jenkins/tools/sp.sd.flywayrunner.installation.FlywayInstallation/flyway_420/flyway -user=$FLYWAY_USER -password=$FLYWAY_PASSWORD -url=$FLYWAY_URL -locations=$FLYWAY_LOCATIONS migrate'            
			    	input message: 'Continue?'
			    }
		        }
		}    
	}
    }
}
