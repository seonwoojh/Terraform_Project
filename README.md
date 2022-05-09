# IaC를 이용하여 Azure 클라우드에 wordpress 배포하기

# 목차

1. 프로젝트 개요 
    1. 프로젝트 목적
    2. 프로젝트 요약
    3. Azure 시스템 아키텍처
        1. 시스템 동작 구조
        2. 시스템 아키텍처

---

[🙋‍♀️ppt 확인하기]([https://github.com/seonwoojh/Terraform_Project/blob/main/IaC 프로젝트_F조_최종본.pdf](https://github.com/seonwoojh/Terraform_Project/blob/main/IaC%20%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8_F%EC%A1%B0_%EC%B5%9C%EC%A2%85%EB%B3%B8.pdf))

# 1. 프로젝트 개요

## a. 프로젝트 목적

AWS 서비스와 IaC 도구인 Ansible과 Terraform의 이해를 돕기위해 Terraform 프로바이더인 AWS 클라우드, Azure 클라우드 각각에 고가용성 wordpress를 배포해보는 프로젝트를 진행한다.

## b. 프로젝트 요약

![Untitled](IaC%E1%84%85%E1%85%B3%E1%86%AF%20%E1%84%8B%E1%85%B5%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%92%E1%85%A1%E1%84%8B%E1%85%A7%20Azure%20%E1%84%8F%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A1%E1%84%8B%E1%85%AE%E1%84%83%E1%85%B3%E1%84%8B%E1%85%A6%20wordpress%20%E1%84%87%E1%85%A2%E1%84%91%E1%85%A9%E1%84%92%205756944bb4ce4a9fa14740c4139393d9/Untitled.png)

프로젝트 일정

![일정표.png](IaC%E1%84%85%E1%85%B3%E1%86%AF%20%E1%84%8B%E1%85%B5%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%92%E1%85%A1%E1%84%8B%E1%85%A7%20Azure%20%E1%84%8F%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A1%E1%84%8B%E1%85%AE%E1%84%83%E1%85%B3%E1%84%8B%E1%85%A6%20wordpress%20%E1%84%87%E1%85%A2%E1%84%91%E1%85%A9%E1%84%92%205756944bb4ce4a9fa14740c4139393d9/%EC%9D%BC%EC%A0%95%ED%91%9C.png)

## c. Azure 시스템 아키텍처

### 시스템 동작 구조

![azure 전체 시스템 동작.png](IaC%E1%84%85%E1%85%B3%E1%86%AF%20%E1%84%8B%E1%85%B5%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%92%E1%85%A1%E1%84%8B%E1%85%A7%20Azure%20%E1%84%8F%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A1%E1%84%8B%E1%85%AE%E1%84%83%E1%85%B3%E1%84%8B%E1%85%A6%20wordpress%20%E1%84%87%E1%85%A2%E1%84%91%E1%85%A9%E1%84%92%205756944bb4ce4a9fa14740c4139393d9/azure_%EC%A0%84%EC%B2%B4_%EC%8B%9C%EC%8A%A4%ED%85%9C_%EB%8F%99%EC%9E%91.png)

전체 시스템 동작 구조는 다음과 같다.
가상머신에서 Packer를 이용해 Ansible Playbook을 실행하여 이미지를 만든다.
Terraform을 이용해 클라우드에 리소스를 생성할 때 VMSS 이미지에 해당 이미지를 사용한다.

**사용한 서비스 목록**

| Vagrant | 가상 시스템 환경을 구축하고 관리하기위한 도구 |
| --- | --- |
| Virtual Machine | 컴퓨터 시스템을 에뮬레이션하는 소프트웨어 |
| Ansible | 오픈 소스 소프트웨어 프로비저닝, 구성 관리, 애플리케이션 전개 IaC 도구 |
| Packer | 클라우드 이미지 및 컨테이너 이미지를 자동으로 빌드하는 도구 |
| Terraform | 인프라스트럭쳐 구축 및 운영의 자동화를 지향하는 오픈 소스 IaC 도구  |
| Visual Studio Code | 디버깅 지원과 Git 제어, 구문 강조 기능등이 포함된 소스 코드 편집기 |

### 시스템 아키텍처

**Azure 클라우드 시스템 아키텍처**

![azure.png](IaC%E1%84%85%E1%85%B3%E1%86%AF%20%E1%84%8B%E1%85%B5%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%92%E1%85%A1%E1%84%8B%E1%85%A7%20Azure%20%E1%84%8F%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A1%E1%84%8B%E1%85%AE%E1%84%83%E1%85%B3%E1%84%8B%E1%85%A6%20wordpress%20%E1%84%87%E1%85%A2%E1%84%91%E1%85%A9%E1%84%92%205756944bb4ce4a9fa14740c4139393d9/azure.png)

전체 아키텍처 개요

Terraform을 이용한 Azure Wordpress 아키텍처는 `Korea Central Region`에 위치하고 있다. 

`WordpressResourcegroup` Resource Group 으로 전체 아키텍처를 구성하는 모든 `Instance`를 관리한다. 

하나의 Resource Group 안에 `가상 네트워크 서비스`를 이용하여 개별 네트워크를 구성함으로써 DB서버와 격리된 환경을 제공한다. 

Zone1, Zone2 두 곳의 `Availability Zone`을 지정하고 `가상 머신 확장 집합(VMSS)`을 통해 Instance를 관리하도록 구성하여 `Single Point of Failure를 방지하고 HA를 확보`하기 위해 설계되었다.

가상 머신 확장 집합 VMSS는 두 곳의 가용성 영역에 각각 `2개씩 총 4개의 VM`을 관리하도록 설계하였다.  여기서 사용된 VM 이미지는 `Packer`와 `Ansible Playbook`을 활용하여 만든 사용자 VM 이미지를 활용한다.

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