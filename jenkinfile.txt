pipeline
{
agent any
stages
{
stage ("contdownload")
{
steps
{
git branch: 'main', url: 'https://github.com/pkr1977/project3.git'   
}
}
stage ("contbuild")
{
steps
{
sh 'mvn package'
}
}
stage ("contdeploy")
{
steps
{
deploy adapters: [tomcat9(credentialsId: '780060ad-27df-4260-aa04-1198a540ff50', path: '', url: 'http://172.31.26.102:8080')], contextPath: 'test', war: '**/*.war'
}
}
stage ("contest")
{
steps
{
sh 'echo "testing pass"'
}
}
stage ("contdelivery")
{
steps
{
deploy adapters: [tomcat9(credentialsId: '780060ad-27df-4260-aa04-1198a540ff50', path: '', url: 'http://172.31.26.102:8080')], contextPath: 'test', war: '**/*.war'
}
}
}
post
{
failure
{
mail bcc: '', body: 'project faild', cc: 'krishna.rp@gmail.com', from: '', replyTo: '', subject: 'project faild', to: 'krishna.rp@gmail.com'
}
}
}
