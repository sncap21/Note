# 마이크로서비스 패턴 실습환경 구성

[ gralde wrapper error ]

gradlew 실행 시 아래와 같은 오류 발생
-- gradle wrapper exception in thread "main" javax.net.ssl.sslhandshakeexception: sun.security.validator.validatorexception: pkix path building failed: sun.security.provider.certpath.suncertpathbuilderexception: unable to find valid certification path to requested target

1. 원인 : gradlew의 download site에 대한 인증서가 local machine 에 존재하지 않음
   * 해당 site : https://downloads.gradle-dn.com <- https://services.gradle.org 에서 redirect 됨
   * gradlew 실행 시 log로는 https://services.gradle.org/distributions/gradle-4.10.2-all.zip 만 표시됨으로
     https://downloads.gradle-dn.com 확인불가
   * wget https://services.gradle.org/distributions/gradle-4.10.2-all.zip 실행 시 redirect site가 표시되며 error 발생됨을 확인
   * https://services.gradle.org 사이트의 인증서는 문제없음
   * web browser로 https://downloads.gradle-dn.com 접속 후 인증서 확인 결과 KOSCOM이 sign 한 인증서로 확인됨
     (당사 방화벽에서 인증서를 교체하는 것으로 보임)
   
2. 해결방안
   * https://downloads.gradle-dn.com 사이트의 인증서를 download : 오픈소스 InstallCert.java compile 하여 실행
   * Certficate Chain 모두 download : jssecacerts 파일에 download됨
   * $JAVA_HOME/jre/lib/security/cacerts 파일을 jssecacerts 파일로 교체 (cacerts 파일 백업 후 copy)
   
[ python get-pip.py 오류 ]

1. 원인 : get-pip.py 내에서 ssl 통신을 수행하는데 인증서 오류 발생 (KOSCOM self signed 인증서)
2. 해결 : pip 를 설치하는 방법은 python get-pip.py 이외에 uuntu는 apt-get install python-pip로 가능 -> 스크립트 수정

[ pip install 오류 ]

1. 원인 : 인증서 오류(KOSCOM self signed 인증서)
2. pip에 argument 추가 : --trusted-host pypi.python.org --trusted-host pypi.org --trusted-host files.pythonhosted.org

# 이후 정상 실행 됨

