# OKE 클러스터 생성 및 k8s deployment 환경설정

 1. Policy 입력
 메뉴위치 : 거버넌스 및 관리 > ID > 정책 > **(버튼클릭) 정책 생성**
 
 정책입력 : **allow service OKE to manage all-resources in tenancy**
 
 루트(Root) 구획에 생성 필수
 생성 후 적용시점까지 몇 분 소요될 수 있음
 ![](resources/images/image01.png)
 
 2. 컨테이너 클러스터(OKE) 생성
 메뉴위치 : 솔루션 및 플랫폼 > 개발자 서비스 > 컨테이너 클러스터(OKE) > **(버튼클릭) 클러스터 생성**
 **빠른 생성** 선택
 가상 클라우드 네트워크 생성 **전용(Private)** 선택
 노드 풀 생성 **구성 : VM.Standard2.1 / 노드 수 : 2**
 KUBERNETES 대시보드 사용 : **체크**
 TILLE(HELM) 사용 : **체크**
 ![](resources/images/image02.png)
