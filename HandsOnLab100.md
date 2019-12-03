# OKE 클러스터 생성 및 Deployment 환경설정



### 1. Policy 입력

메뉴위치 : 거버넌스 및 관리 > ID > 정책 > **`정책 생성`** 버튼 클릭

- 정책입력 : **``allow service OKE to manage all-resources in tenancy``**

- 루트(Root) 구획에 생성 필수

- 생성 후 적용시점까지 몇 분 소요될 수 있음

![](resources/images/image01.png)



### 2. 컨테이너 클러스터(OKE) 생성

메뉴위치 : 솔루션 및 플랫폼 > 개발자 서비스 > 컨테이너 클러스터(OKE) > **`클러스터 생성`** 버튼 클릭

- **빠른 생성** 선택

- 가상 클라우드 네트워크 생성 **전용(Private)** 선택

- 노드 풀 생성 **구성 : VM.Standard2.1 / 노드 수 : 2**

- KUBERNETES 대시보드 사용 : **체크**

- TILLE(HELM) 사용 : **체크**

![](resources/images/image02.png)



### 3. Oracle Cloud CLI 설치 및 Configuration

OCI-CLI 설치 참고 : https://docs.cloud.oracle.com/iaas/Content/API/SDKDocs/cliinstall.htm

```
[opc@test ~]$ bash -c "$(curl -L https://raw.githubusercontent.com/oracle/oci-cli/master/scripts/install/install.sh)"
...
...
===> In what directory would you like to place the install? (leave blank to use '/home/opc/lib/oracle-cli'):   <-- 엔터
...
===> In what directory would you like to place the 'oci' executable? (leave blank to use '/home/opc/bin'):   <-- 엔터
...
===> In what directory would you like to place the OCI scripts? (leave blank to use '/home/opc/bin/oci-cli-scripts'):   <-- 엔터
...
===> Currently supported optional packages are: ['db (will install cx_Oracle)']
What optional CLI packages would you like to be installed (comma separated names; press enter if you don't need any optional packages)?:   <-- 엔터
...
===> Modify profile to update your $PATH and enable shell/tab completion now? (Y/n): y
===> Enter a path to an rc file to update (leave blank to use '/home/opc/.bashrc'):   <-- 엔터
...
...

[opc@test ~]$ oci -v
2.6.14
```

OCI-CLI config 생성

```
[opc@test ~]$ oci setup config
...
Enter a location for your config [/home/opc/.oci/config]:   <-- 엔터
Enter a user OCID:  <-- 클라우드 콘솔에서 user OCID 복사 (아래 이미지 참고)
Enter a tenancy OCID:   <-- 클라우드 콘솔에서 tenancy OCID 복사 (아래 이미지 참고)
Enter a region (e.g. ap-mumbai-1, ap-seoul-1, ap-sydney-1, ap-tokyo-1, ca-toronto-1, eu-frankfurt-1, eu-zurich-1, sa-saopaulo-1, uk-gov-london-1, uk-london-1, us-ashburn-1, us-gov-ashburn-1, us-gov-chicago-1, us-gov-phoenix-1, us-langley-1, us-luke-1, us-phoenix-1): ap-seoul-1
Do you want to generate a new RSA key pair? (If you decline you will be asked to supply the path to an existing key.) [Y/n]: y
Enter a directory for your keys to be created [/home/opc/.oci]:
Enter a name for your key [oci_api_key]:
Public key written to: /home/opc/.oci/oci_api_key_public.pem
Enter a passphrase for your private key (empty for no passphrase):
Private key written to: /home/opc/.oci/oci_api_key.pem
Fingerprint: e5:ef:17:98:6e:b3:e1:64:01:cc:46:85:7b:58:93:0b
Config written to /home/opc/.oci/config
...
...

[opc@test ~]$ ls .oci
config  oci_api_key.pem  oci_api_key_public.pem
```

- user OCID 확인방법

  프로파일 > 사용자명 클릭

  ![](resources/images/image03.png)

  사용자 세부정보 > OCID 복사 클릭

  ![](resources/images/image04.png)

- tenancy OCID 확인방법

  프로파일 > 테넌시: xxx 클릭

  ![](resources/images/image05.png)

  테넌시 정보 > OCID 복사 클릭

  ![](resources/images/image06.png)



### 4. Kubectl 설치

참고 : https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-on-linux



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
