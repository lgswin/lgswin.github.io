---
layout: post
title: "AWS Amplify"
date: 2025-4-3
categories: [Awsamplify]
tags: [Awsamplify]
---

# **🧭 Hitchhikers - AWS Architecture**

---

**🌟 1. 핵심 기능 요약**

**앱 기능:**

- 회원가입 / 로그인 (인증)
- 일정 등록 / 보기
- 사용자 프로필
- 유저 리스트 관리
- 맵 연동
- 알림 기능

**🚀 2. 사용하는 AWS 서비스 (Amplify 기반)**

| 기능 | AWS서비스 | Amplify모듈 |
| --- | --- | --- |
| 인증 | Cognito | amplify add auth |
| 데이터저장 | DynamoDB | amplify add storage |
| 백엔드로직 | Lambda | amplify add function |
| API 연동 | API gateway | amplify add api |
| 지도 기능 | AWS Location Service + MapLibre | amplify add geo |
| 파일 업로드 | S3 | amplify add storage |
| 호스팅 | Amplify hosting | amplify add hosting |

**🔐 3. 인증: Amplify + Cognito**

**✅ 구현 방법**

`amplify add auth`

• 이메일 기반 회원가입 / 로그인

• Cognito User Pool 자동 생성

• 프론트엔드에 Auth 모듈로 바로 연동 가능

**✅ 프론트 코드 예시**

```jsx
import { Auth } from 'aws-amplify';

await Auth.signUp({
  username: 'gsl@example.com',
  password: 'Password123!',
  attributes: { email: 'gsl@example.com' }
});

await Auth.signIn('gsl@example.com', 'Password123!');
```

**✅ 유저 관리는 어디서?**

•	AWS Cognito 콘솔 (또는 Admin API)에서 유저 조회/삭제/그룹 관리

---

**🛠️ 4. 일정 등록/유저 데이터 처리: Lambda + DynamoDB 연동**

**✅ 전체 흐름**

```jsx
React/Vue App
   ↓
API Gateway (REST)
   ↓
Lambda Function
   ↓
DynamoDB
```

**✅ Step-by-step 구성**

**📌 1) Lambda 함수 추가**

`amplify add function`

•	예: createScheduleFn

•	Node.js 기반 Lambda 생성

•	DynamoDB 접근 권한 부여 (Yes)

**📌 2) DynamoDB 테이블 생성**

`amplify add storage`

•	NoSQL (DynamoDB) 선택

•	예: ScheduleTable

•	Partition Key: scheduleId

•	IAM 권한 설정 (Lambda에서 읽기/쓰기 허용)

**📌 3) Lambda 코드 예시**

```jsx
const AWS = require('aws-sdk');
const docClient = new AWS.DynamoDB.DocumentClient();

exports.handler = async (event) => {
  const data = JSON.parse(event.body);

  const params = {
    TableName: process.env.STORAGE_SCHEDULETABLE_NAME,
    Item: {
      scheduleId: data.scheduleId,
      userId: data.userId,
      title: data.title,
      date: data.date,
    },
  };

  await docClient.put(params).promise();
  return {
    statusCode: 200,
    body: JSON.stringify({ message: 'Saved!' }),
  };
};
```

> ⚠️ 환경변수 STORAGE_SCHEDULETABLE_NAME는 Amplify가 자동 설정
> 

**📌 4) API Gateway 연결**

`amplify add api`

- REST API 선택
- Lambda 연동 : createSchedulefn
- 엔트리포인트 : /schedule

**🌐 5. 프론트엔드에서 API 호출**

```jsx
import { API } from 'aws-amplify';

await API.post('scheduleAPI', '/schedule', {
  body: {
    scheduleId: '001',
    userId: 'gunsoo',
    title: 'Ride to Toronto',
    date: '2025-04-10'
  }
});
```

**📦 6. 추가 기능 제안 및 연동 방법**

| 기능 | 서비스 | 구성방법 |
| --- | --- | --- |
| 프로필 사진 업로드 | s3 | amplify add storage (Content) + Storage.put() 사용 |
| 지도 연동 | AWS Location Service | amplify add geo + MapLibre JS 연동 |
| 푸시 알림 | SNS 또는 FCM | Lambda + Firebase Admin SDK or SNS trigger |
| 권한 구분 | Cognito 그룹 | amplify update auth → 그룹 생성 → 유저 역할 분리 |

**🚀 7. 배포 및 관리**

**전체 리소스 배포:**

`amplify push`

**코드 + 인프라 모두 Git으로 버전 관리 가능**

•	/amplify/backend/ 아래에 CloudFormation 기반 설정 자동 저장 (IaC)

---

**✅ 최종 구조 정리 (도식)**

```jsx
[React / Vue 앱]
       ↓
   Amplify API (REST)
       ↓
  Lambda Function (백엔드 로직)
       ↓
   DynamoDB (데이터 저장)

[Amplify Auth]
  ↳ Cognito (유저 인증 / 그룹 관리)

[Amplify Storage]
  ↳ S3 (파일 업로드)
  ↳ DynamoDB (NoSQL 저장소)

[지도 연동]
  ↳ AWS Location Service (Geo)

[CI/CD]
  ↳ Amplify Hosting + GitHub 연동
```

**🏁 결론**

Gunsoo님의 Hitchhikers 앱에 AWS Amplify를 사용하면,

✅ 인증부터

✅ 서버리스 백엔드

✅ 실시간 데이터 저장

✅ 파일 관리

✅ 지도 기능