#!groovy
@Library('jenkinslib') _
def tools = new org.devops.tools()

String workspace = "/opt/jenkins/worksapce"
//pipeline
pipeline{
    parameters { string(name: 'DEPLOY_ENV', defaultValue: 'staging', description: '') }
    agent{
        node {
            label "master"   //指定运行节点的标签或者名称
            customWorkspace "${workspace}"   //指定运行工作目录（可选）
        }
    }
    
    options{
        timestamps()      //日志会有时间
        skipDefaultCheckout()       //删除隐式checkout scm语句
        disableConcurrentBuilds()    //禁止并行
        timeout(time:1, unit:'HOURS') //流水线超时设置1h
    }
    
    
    stages{
       
        // 下载代码
        stage("GetCode"){   // 阶段名称
            when { environment name: 'test', value: 'abcd' }
            steps{ //步骤
                timeout(time:5, unit:'MINUTES'){    //步骤超时时间限制
                    script{ //填写运行代码
                        println("获取代码")
                        println("${test}")
                        println("${DEPLOY_ENV}")   //指定运行工作目录（可选）
                        input id: 'Test', message: 'Should we continue?', ok: 'Yes, we should.', parameters: [choice(choices: ['a', 'b'], description: 'a or b', name: 'good')], submitter: 'admin'
                    }
                }
            }
        }    
        stage("01"){
            failFast true
            parallel{

                //构建
                stage("Build"){   
                    steps{ 
                        timeout(time:20, unit:'MINUTES'){    
                            script{ 
                                println("应用打包")
                                mvnHome = tool "m2"
                                println(mvnHome)
                                sh "${mvnHome}/bin/mvn --version"
                            }
                        }
                    }
                }
                
                //代码扫描
                stage("CodeScan"){   
                    steps{ 
                        timeout(time:20, unit:'MINUTES'){    
                            script{ 
                                println("代码扫描")
                                tools.PrintMes("this is my lib!")
                            }
                        }
                    }
                }
            }
    }
    }
    
    
    //构建后操作
    post{
        always{      // always{}总是执行脚本片段
            script{
                println("always")
            }
        }
        
        success{      // success{}执行成功后执行
            script{
                currentBuild.description = "\n 构建成功！"     //currentBuild是一个全局变量，description构建描述
            }
        }
        
        failure{      // failure{}执行失败后执行
            script{
                currentBuild.description = "\n 构建失败！"     
            }
        }
        
        aborted{      // aborted{}执行被取消后执行
            script{
                currentBuild.description = "\n 构建取消！"     
            }
        }
    }
}
