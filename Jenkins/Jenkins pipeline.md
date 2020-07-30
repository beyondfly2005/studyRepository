# Pipeline 流水线任务



### pipeline脚本

```groovy
#!groovy

pipline {
    agent {node {label 'master'}}
    
    environment {
        path='/bin:/sbin:/usr/bin:/usr/sbin/:usr/local/bin'
    }
    parameters {
        choice(
            choices:'dev\nprod',
            description: 'choose depol environment',
            name: 'deply_env'
                
        )
        String  (name: 'version', defaultValue: '1.0.0', description: 'build version')
     }
    stages {
        stage("Checkout test repo") {
            steps{
                sh 'git config --global http.sslVerify false'
                dir("${env.WORKSPACE}"){
                    gt branch: 'master', credentialsID:"", url;'https://root@gitlab /git'
                }
            }
        }
        stage("Print env variable"){
            steps {
                dir ("${emv.WORKSPACE}")
                sh """
                echo "[INFO] Pring env variable"
                echo "Current deployment "
                echo "The build is $"
                echo "[INFO] Donw..."
                """
            }
        }
        stage("Check test Properties"){
            steps{
                dir ("${env.WORKSPACE}")
                sh """
                echo “[INFO] Check test.properties"
                if [ -s test.properties]
                then
                	cat test.properties
                	echo "[INFO] Do"
                else
                	echo "test.properties is enpty"
                	fi
                """
            }
        }
    }
}
```








### svn+maven部署项目pipeline脚本方式

```groovy
pipeline {
    agent any
    environment {

        def sscs_ms_version = "SSCS.MS_V100R001B040"

        def iCommunity_Dir = "/JenkinsPackage/iCommunity/SSCS.MS/"
        def iCommunity_WorkSpace_Dir = "/var/lib/jenkins/workspace/智慧社区_01.sscs.ms"
    }
    stages  {
        stage("检出智慧社区相关代码") {
            steps {
                echo "开始检出 SSCS_MS 代码"
                checkout([$class: 'SubversionSCM',
                    additionalCredentials: [],
                    excludedCommitMessages: '',
                    excludedRegions: '',
                    excludedRevprop: '',
                    excludedUsers: '',
                    filterChangelog: false,
                    ignoreDirPropChanges: false,
                    includedRegions: '',
                    locations: [[credentialsId: 'svn_readonly', #新建SVN凭据时填写的ID
                       depthOption: 'infinity',
                       ignoreExternalsOption: true,
                       local: 'SSCS_MS',
                       remote: "http://192.168.1.3/CoNET/view/trunk/sscp/sscs-ms"]],
                    workspaceUpdater: [$class: 'UpdateUpdater']])
                echo "检出 SSCS_MS 成功"
            }
        }

        stage("构建智慧社区相关模块") {
            tools{jdk "JDK1.8"}
            steps {
                echo "开始构建 SSCS_MS 模块"
                dir('SSCS_MS') {
                    sh 'mvn -X package'
                }
                echo "构建 SSCS_MS 成功"
            }
        }

        stage("复制各个模块到指定目录") {
            steps {
                sh 'mkdir -p ${iCommunity_Dir}${BUILD_NUMBER}'

                sh 'mkdir -p ${iCommunity_Dir}${BUILD_NUMBER}/${sscs_ms_version}'

                dir('SSCS_MS/target') {
                    sh 'cp ${sscs_ms_version}.zip ${iCommunity_Dir}${BUILD_NUMBER}/${sscs_ms_version}/'
                }
                echo "sscs_ms的包移动成功！"
            }
        }
    }
}
```









