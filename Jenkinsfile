node('maven') {
  stage('Build') {
    git url: "https://github.com/tonyfieit/cart-service.git"
    sh "mvn package"
    stash name:"jar", includes:"target/cart-1.0.jar"
  }
 stage('Build Image') {
    unstash name:"jar"
    sh "oc start-build cart --from-file=target/cart-1.0.jar --follow"
  }
  stage('Deploy') {
    openshiftDeploy depCfg: 'cart'
    openshiftVerifyDeployment depCfg: 'cart', replicaCount: 1, verifyReplicaCount: true
  }
  stage('System Test') {
    sh "curl -s -X POST http://cart:8080/api/cart/dummy/666/1"
    sh "curl -s http://cart:8080/api/cart/dummy | grep 'Dummy Product'"
  }
}
