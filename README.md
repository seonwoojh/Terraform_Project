# <center> IaC를 이용하여 Azure 클라우드에 wordpress 배포하기 <center/>

프로젝트에 대한 자세한 내용은 아래 링크에서 확인하실 수 있습니다.

<br/>


[🙋‍♀️ppt 확인하기](https://github.com/seonwoojh/Terraform_Project/blob/main/IaC%20%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8_F%EC%A1%B0_%EC%B5%9C%EC%A2%85%EB%B3%B8.pdf)

<br/>
    
[🙋‍♀️프로젝트 문서(Markdown) 확인하기](https://github.com/seonwoojh/Terraform_Project/blob/main/IaC%EB%A5%BC_%EC%9D%B4%EC%9A%A9%ED%95%98%EC%97%AC_Azure_%ED%81%B4%EB%9D%BC%EC%9A%B0%EB%93%9C%EC%97%90_wordpress_%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B8%B0.pdf)

<br/>

# 1. 프로젝트 개요

<br/>

## a. 프로젝트 목적

AWS 서비스와 IaC 도구인 Ansible과 Terraform의 이해를 돕기위해 Terraform 프로바이더인 AWS 클라우드, Azure 클라우드 각각에 고가용성 wordpress를 배포해보는 프로젝트를 진행한다.
    
<br/>
    
## b. 프로젝트 요약

![Untitled](https://github.com/seonwoojh/Terraform_Project/blob/main/images/Untitled.png)

<br/>
   
## 프로젝트 일정

<br/>

![일정표](https://github.com/seonwoojh/Terraform_Project/blob/main/images/%EC%9D%BC%EC%A0%95%ED%91%9C.png)

<br/>
    
## c. Azure 시스템 아키텍처

<br/>
    
### 시스템 동작 구조
    
<br/>
    
![azure 전체 시스템 동작.png](https://github.com/seonwoojh/Terraform_Project/blob/main/images/azure_%EC%A0%84%EC%B2%B4_%EC%8B%9C%EC%8A%A4%ED%85%9C_%EB%8F%99%EC%9E%91.png)
    
<br/>
    
전체 시스템 동작 구조는 다음과 같다.
가상머신에서 Packer를 이용해 Ansible Playbook을 실행하여 이미지를 만든다.
Terraform을 이용해 클라우드에 리소스를 생성할 때 VMSS 이미지에 해당 이미지를 사용한다.
    
<br/>
    
**사용한 서비스 목록**
    
<br/>
    
| Vagrant | 가상 시스템 환경을 구축하고 관리하기위한 도구 |
| --- | --- |
| Virtual Machine | 컴퓨터 시스템을 에뮬레이션하는 소프트웨어 |
| Ansible | 오픈 소스 소프트웨어 프로비저닝, 구성 관리, 애플리케이션 전개 IaC 도구 |
| Packer | 클라우드 이미지 및 컨테이너 이미지를 자동으로 빌드하는 도구 |
| Terraform | 인프라스트럭쳐 구축 및 운영의 자동화를 지향하는 오픈 소스 IaC 도구  |
| Visual Studio Code | 디버깅 지원과 Git 제어, 구문 강조 기능등이 포함된 소스 코드 편집기 |
    
<br/>
    
### 시스템 아키텍처
    
<br/>
    
**Azure 클라우드 시스템 아키텍처**

![azure.png](https://github.com/seonwoojh/Terraform_Project/blob/main/images/azure.png)

<br/>

### 전체 아키텍처 개요

Terraform을 이용한 Azure Wordpress 아키텍처는 `Korea Central Region`에 위치하고 있다. 

`WordpressResourcegroup` Resource Group 으로 전체 아키텍처를 구성하는 모든 `Instance`를 관리한다. 

하나의 Resource Group 안에 `가상 네트워크 서비스`를 이용하여 개별 네트워크를 구성함으로써 DB서버와 격리된 환경을 제공한다. 

Zone1, Zone2 두 곳의 `Availability Zone`을 지정하고 `가상 머신 확장 집합(VMSS)`을 통해 Instance를 관리하도록 구성하여 `Single Point of Failure를 방지하고 HA를 확보`하기 위해 설계되었다.

가상 머신 확장 집합 VMSS는 두 곳의 가용성 영역에 각각 `2개씩 총 4개의 VM`을 관리하도록 설계하였다.  여기서 사용된 VM 이미지는 `Packer`와 `Ansible Playbook`을 활용하여 만든 사용자 VM 이미지를 활용한다.
    
<br/>
    
**사용한 Azure 서비스 목록**

| Region | Korea Central |
| --- | --- |
| Resource Group | 리소스 그룹 |
| Vnet | 가상 네트워크 |
| Availability Zone | 가용성 영역 |
| Application Gateway | 애플리케이션 게이트웨이 |
| NSG | 네트워크 보안 그룹 |
| NAT Gateway | NAT 게이트웨이 |
| VM | Bastion Host Virtual Machine |
| VMSS | 가상 머신 확장 집합 |
| Azure Database for MariaDB Server | Azure 관리형 데이터베이스 서비스(Maria DB) |

---
