





### 5. Kubernetes config 파일 액세스

### 5. Kubernetes config 파일 액세스

핸즈온 과정에서 진행하게 될 MAS 기반의 Web Application 을 배포할 namespace 를 생성합니다.

Kubernetes 에서 namespace 는 논리적인 구획입니다.

```
ubuntu@test:~$ kubectl create namespace travel-agency
```



### 6. Kubernetes secret 생성

위에서 생성한 namespace에서 사용할 secret을 생성합니다.

> secret은 보안이 중요한 패스워드나, API 키, 인증서 파일들은 secret에 저장할 수 있습니다. secret은 항상 메모리에 저장되어 있기 때문에 상대적으로 접근이 어렵습니다. 하나의 secret의 사이즈는 최대 1M까지 지원되는데, 메모리에 지원되는 특성 때문에, secret을 여러개 저장하게 되면 API Server나 노드에서 이를 저장하는 kubelet의 메모리 사용량이 늘어나서 Out Of Memory와 같은 이슈를 유발할 수 있기 때문에, 보안적으로 꼭 필요한 정보만 secret에 저장하도록 하는게 좋습니다.

```
ubuntu@test:~$ kubectl create secret docker-registry ocirsecret --docker-server=<region-code>.ocir.io --docker-username='<tenancy-namespace>/<oci-username>' --docker-password='<oci-auth-token>' --docker-email='<email-address>'
```