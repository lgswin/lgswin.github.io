---
layout: post
title: "WordPress installation on GCP"
date: 2025-1-10
categories: [WordPress]
tags: [WordPress, Homepage, GCP]
---

## 1. To make the external ip static

#### 1) GCP console > VPC network > external ip address
#### 2) More action of the ip that want to fix > Promote to the static ip address
#### 3) After completion of the process, the type will be seen as static

Ephemeral(임시) IP 주소를 사용 중이므로 인스턴스 재부팅 시 외부 IP가 변경될 수 있습니다.
고정 IP 할당을 통해 외부 IP가 변경되지 않도록 설정하세요:


---
## 2. 방화벽 규칙 점검

GCP 방화벽 설정이 HTTP(포트 80) 및 HTTPS(포트 443) 트래픽을 허용하도록 제대로 구성되어 있는지 확인하세요:
•	GCP Console > VPC 네트워크 > 방화벽 규칙으로 이동.
•	http-server 및 https-server 태그가 올바르게 적용되어 있는지 확인.
•	적용되지 않았다면, 네트워크 태그를 인스턴스에 추가하세요:
1.	Compute Engine > VM 인스턴스 > wp-instance-1 클릭.
2.	편집 > 네트워크 태그에 http-server, https-server 추가.