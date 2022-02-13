# 通过 Jenkins 定期自动给老板提供 Git 仓库的多维度代码分析报告

[blog.51cto.com](https://blog.51cto.com/shenxianpeng/3060087)

1.  pipeline{
    
2.  agent{
    
3.  node {
    
4.  label 'main-slave'
    
5.  customWorkspace "/workspace/gitstats"
    
6.  }
    
7.  }
    

9.  environment {
    
10.  USER\_CRE = credentials("d1cbab74-823d-41aa-abb7")
    
11.  webapproot = "/workspace/apache-tomcat-7.0.99/webapps/gitstats"
    
12.  }
    

14.  parameters {
    
15.  booleanParam(defaultValue: true, name: 'repo1', description: 'uncheck to disable \[repo1\]')
    
16.  booleanParam(defaultValue: true, name: 'repo2', description: 'uncheck to disable \[repo2\]')
    
17.  }
    

19.  triggers {
    
20.  cron '0 3 \* \* 7' # 每周日早上进行定时运行，因此此时机器是空闲的。
    
21.  }
    

23.  options {
    
24.  buildDiscarder(logRotator(numToKeepStr:'10'))
    
25.  }
    

27.  stages{
    
28.  stage("Checkout gitstats"){
    
29.  steps{
    
30.  \# 准备存放 html 报告目录
    
31.  sh "mkdir -p html"
    
32.  \# 下载 gitstats 代码
    
33.  sh "rm -rf gitstats && git clone https://github.com/hoxu/gitstats.git"
    
34.  }
    
35.  }
    
36.  stage("Under statistics") {
    
37.  parallel {
    
38.  stage("reop1") {
    
39.  when {
    
40.  expression { return params.repo1 } # 判断是否勾选了
    
41.  }
    
42.  steps {
    
43.  \# 下载要进行分析的仓库 repo1
    
44.  sh 'git clone -b master https://$USER\_CRE\_USR:"$USER\_CRE\_PSW"@git.software.com/scm/repo1.git'
    
45.  \# 进行仓库 repo1 的历史记录分析
    
46.  sh "cd gitstats && ./gitstats ../repo1 ../html/repo1"
    
47.  }
    
48.  post {
    
49.  success {
    
50.  \# 如果分析成功，则将分析结果放到 apache-tomcat-7.0.99/webapps/gitstats 目录下
    
51.  sh 'rm -rf ${webapproot}/repo1 && mv html/repo1 ${webapproot}'
    
52.  \# 然后删掉 repo1 的代码和 html 报告，以免不及时清理造成磁盘空间的过度占用
    
53.  sh "rm -rf repo1"
    
54.  sh "rm -rf html/repo1"
    
55.  }
    
56.  }
    
57.  }
    
58.  }
    
59.  stage("repo2") {
    
60.  when {
    
61.  expression { return params.repo2 }
    
62.  }
    
63.  steps {
    
64.  sh 'git clone -b master https://$USER\_CRE\_USR:"$USER\_CRE\_PSW"@git.software.com/scm/repo2.git'
    
65.  sh "cd gitstats && ./gitstats ../repo2 ../html/repo2"
    
66.  }
    
67.  post {
    
68.  success {
    
69.  sh 'rm -rf ${webapproot}/repo2 && mv html/repo2 ${webapproot}'
    
70.  sh "rm -rf repo2"
    
71.  sh "rm -rf html/repo2"
    
72.  }
    
73.  }
    
74.  }
    
75.  }
    
76.  }
    
77.  }
    
78.  post{
    
79.  always{
    
80.  \# 总是给执行者分送邮件通知，不论是否成功都会对工作空间进行清理
    
81.  script {
    
82.  def email = load "vars/email.groovy"
    
83.  email.build(currentBuild.result, '')
    
84.  }
    
85.  cleanWs()
    
86.  }
    
87.  }
    
88.  }
    

[查看原网页: blog.51cto.com](https://blog.51cto.com/shenxianpeng/3060087)