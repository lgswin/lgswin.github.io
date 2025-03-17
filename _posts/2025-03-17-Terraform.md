---
layout: post
title: "About Terraform"
date: 2025-3-17
categories: [IaC]
tags: [Terraform]
---

### **1. Terraform 이란?**

•	HashiCorp에서 개발한 오픈소스 IaC 도구

•	AWS, Azure, GCP 같은 클라우드 서비스의 인프라를 코드로 정의하고 실행 가능

•	선언적(Declarative) 방식: 원하는 최종 상태를 코드로 작성하면 Terraform이 알아서 적용

---

### **2. Workflows**

`terraform init`

    - Terraform을 처음 실행할 때 필요한 Provider 플러그인 다운로드

`terraform plan`

    - 행 전, 변경될 내용을 미리 확인하는 단계

`terraform apply`

    - 실제로 변경 사항을 적용하여 리소스를 생성/수정/삭제

`terraform destroy`

    - 모든 인프라 리소스를 제거


### **3. Terraform 핵심 개념**

`provider` 

#### 대표적인 Provider 예시:
    •	AWS (hashicorp/aws)
    •	Azure (hashicorp/azurerm)
    •	Google Cloud (hashicorp/google)
    •	Kubernetes (hashicorp/kubernetes)
    •	Docker (kreuzwerker/docker)

```
provider "aws" {
  region = "us-east-1" # aws region
}
```

`resource`


#### Terraform이 관리하는 실제 인프라 구성 요소

	•	EC2 인스턴스 (가상 서버): aws_instance
	•	S3 버킷 (저장소): aws_s3_bucket
	•	RDS 데이터베이스: aws_db_instance
	•	IAM 사용자: aws_iam_user

```
resource "aws_instance" "example" { # "example": EC2 인스턴스 생성
  ami  = "ami-0c55b159cbfafe1f0"    # Amazon Linux 2 AMI 이미지 생성
  instance_type = "t2.micro"        # 서버 사양 설정
  tags = {                          # 태그 설정
    Name = "MyInstance"
  }
}
```


`state`

Terraform은 현재 관리하는 인프라의 상태를 “terraform.tfstate” 파일에 저장합니다.
	•	Terraform이 적용한 리소스를 추적하고 변경 사항을 감지하는 역할
	•	상태 파일을 관리해야 불필요한 리소스 변경을 방지할 수 있음
	•	팀 프로젝트에서는 원격 상태 저장 (Remote State, 예: S3 + DynamoDB) 을 활용하여 공유 가능


`variables`

변수(Variables)는 Terraform 코드에서 값을 동적으로 설정하는 데 사용됩니다.
코드의 재사용성과 가독성을 높이는 역할을 합니다.

```
variable "instance_type" {
  default = "t2.micro"      # 기본값을 "t2.micro" 로 설정
}

resource "aws_instance" "example" {
  instance_type = var.instance_type  # 변수 사용
}
```

`output`

Terraform에서 리소스가 생성된 후 중요한 정보를 출력할 수 있습니다.

```
output "instance_ip" {
  value = aws_instance.example.public_ip
}
```


------

### S3 bucket

```
provider "aws" {
  region = "us-east-1"
}

# Create 4 S3 Buckets with No Public Access
resource "aws_s3_bucket" "private_buckets" {
  count  = 4
  bucket = "gunsoo-private-bucket-${count.index}"  # Change prefix to your own unique name
}

# Block Public Access for all Buckets
resource "aws_s3_bucket_public_access_block" "block_public_access" {
  count  = 4
  bucket = aws_s3_bucket.private_buckets[count.index].id

  block_public_acls       = true
  block_public_policy     = true
  ignore_public_acls      = true
  restrict_public_buckets = true
}
```