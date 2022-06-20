
#### Jenkinsfile for Quick Task
================================
```
pipeline {
    agent any

  stages {
    stage('Initial cleanup') {
      steps {
        dir('${WORKSPACE}') {
          deleteDir()
       } 
      }
    }

  stage('Checkout SCM') {
      steps{
            git branch: 'feature/jenkinspipeline-stages', url: 'https://github.com/mexez/P14_CI-with-Jenkins-Ansible-Artifactory-Sonarqube-PHP.git'
         }
       }

  stage('Build') {
    steps {
      script {
        sh 'echo "Building Stage"'
        }
      }
    }

  stage('Test') {
    steps {
      script {
          sh 'echo "Testing Stage"'
        }
      }
    }

  stage('Package') {
    steps {
      script {
          sh 'echo "Packaging App"'
        }
      }
    }

  stage('Deploy') {
    steps {
      script {
          sh 'echo "Deploying to Dev"'
        }
      }
    }
    
  stage('clean up'){
    steps {
      cleanWs()
    } 
   }
   
    }
}
```

#### ansible.cfg files
=========================
```
[defaults]
timeout = 160
callback_whitelist = profile_tasks
log_path=~/ansible.log
host_key_checking = False
gathering = smart
ansible_python_interpreter=/usr/bin/python3
allow_world_readable_tmpfiles=true


[ssh_connection]
ssh_args = -o ControlMaster=auto -o ControlPersist=30m -o ControlPath=/tmp/ansible-ssh-%h-%p-%r -o ServerAliveInterval=60 -o ServerAliveCountMax=6
```

==============================

# Install php and its modules
=====================
        sudo su - (sudo login)
yum module reset php -y
yum module enable php:remi-7.4 -y
yum install -y php php-common php-mbstring php-opcache php-intl php-xml php-gd php-curl php-mysqlnd php-fpm php-json
systemctl start php-fpm
systemctl enable php-fpm

Install composer
=====================================
curl -sS https://getcomposer.org/installer | php
sudo mv composer.phar /usr/bin/composer
   Verify Composer is installed or not
composer --version

### Artifactory
================


#### for database connection
=============================

sudo yum install mysql (install mysql client)
sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
DB_CONNECTION=mysql
DB_PORT=3306

sudo systemctl restart mysql
sudo systemctl status mysql
