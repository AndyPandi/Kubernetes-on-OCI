### 1. Kubernetes 대시보드 접속

이전 실습을 참고해서 Kubernetes 대시보드에 접속



### 2. Pod 생성

샘플 웹애플리케이션 travel-agency 메인페이지 Pod 생성

+[[kubernetes-deployment.yaml 파일 다운로드]](resources/kubernetes-deployment.yaml)



### 3. Service 생성

샘플 웹애플리케이션 travel-agency 메인페이지 접근을 위한 Service 생성

+[[kubernetes-svc.yaml 파일 다운로드]](resources/kubernetes-svc.yaml)



### 4. 샘플 웹 애플리케이션 접속

Local PC에서 웹브라우저 실행 후 해당 서비스 IP:Port 로 접속

````
ex) 145.2.5.155:9004
````

