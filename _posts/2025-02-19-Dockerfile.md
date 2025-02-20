---
layout: post
title: "About Dockerfile"
date: 2025-2-19
categories: [Container]
tags: [Docker]
---

### **1. Dockerfile 개요**

Dockerfile은 도커(Docker) 이미지를 생성하기 위한 설정 파일입니다. 이 파일에는 애플리케이션을 컨테이너화하기 위해 필요한 환경 설정, 패키지 설치, 소스 코드 추가, 실행 명령어 등이 포함됩니다.

Dockerfile을 기반으로 `docker build` 명령을 실행하면 이미지가 생성되며, 이 이미지를 사용하여 컨테이너를 실행할 수 있습니다.

---

### **2. Dockerfile의 주요 명령어**

Dockerfile은 여러 가지 명령어로 구성되며, 주로 아래와 같은 명령어들이 사용됩니다.

### **(1) FROM** - 기본 이미지 지정

```
FROM ubuntu:latest
```

- 도커 이미지는 계층적 구조를 가지며, `FROM` 명령어는 기본(Base) 이미지를 지정합니다.
- 위 예제에서는 `ubuntu:latest` 이미지를 기반으로 새로운 이미지를 생성합니다.

---

### **(2) LABEL** - 이미지 메타데이터 추가

```
LABEL maintainer="gunsoo@example.com
```

- `LABEL` 명령어를 사용하여 이미지에 메타데이터(설명, 버전 정보 등)를 추가할 수 있습니다.

---

### **(3) WORKDIR** - 작업 디렉토리 설정

```
WORKDIR /a
```

- 컨테이너 내부에서 명령을 실행할 기본 디렉토리를 설정합니다.
- 이후 명령어들은 `/app` 디렉토리를 기준으로 실행됩니다.

---

### **(4) COPY & ADD** - 파일 및 디렉토리 복사

```
COPY ./src /app
ADD https://example.com/sample.zip /app/sample.zip
```

- `COPY`: 로컬 시스템의 파일 또는 디렉토리를 컨테이너 내부로 복사합니다.
- `ADD`: `COPY`와 유사하지만, URL에서 파일을 직접 다운로드하거나, 압축된 파일을 자동으로 해제하는 기능이 있습니다.

---

### **(5) RUN** - 명령 실행

```
RUN apt-get update && apt-get install -y python3
```

- 컨테이너가 생성될 때 실행할 명령어를 지정합니다.
- 주로 패키지 설치 및 환경 설정을 수행합니다.
- `RUN` 명령어를 실행하면 새로운 이미지 레이어가 생성됩니다.

---

### **(6) ENV** - 환경 변수 설정

```
ENV APP_ENV=production
```

- 컨테이너 실행 시 환경 변수를 설정합니다.
- `docker run -e APP_ENV=development` 명령어로 실행할 때 환경 변수를 변경할 수도 있습니다.

---

### **(7) EXPOSE** - 포트 개방

```
EXPOSE 8080
```

- 컨테이너에서 사용할 포트를 지정합니다.
- `EXPOSE` 자체는 포트를 열어주는 기능이 아니라, 컨테이너를 실행할 때 `p` 옵션과 함께 사용해야 합니다.
    
    ```bash
    docker run -p 8080:8080 my-app
    ```
    

---

### **(8) CMD & ENTRYPOINT** - 컨테이너 실행 명령

```
CMD ["python3", "app.py"]
ENTRYPOINT ["python3", "app.py"
```

- `CMD`: 컨테이너가 실행될 때 기본적으로 실행될 명령을 지정합니다.
- `ENTRYPOINT`: `CMD`와 유사하지만, `docker run` 명령에서 추가 인자를 줄 경우, `ENTRYPOINT`는 고정된 실행 파일이고 `CMD`는 인자로 사용됩니다.

**예시 비교**

```
CMD ["python3", "app.py"]
# 실행: docker run my-container
# 실행 명령: python3 app.py

ENTRYPOINT ["python3"]
CMD ["app.py"]
# 실행: docker run my-container myscript.py
# 실행 명령: python3 myscript.py
```

---

### **3. Dockerfile 예제**

### **(1) 간단한 Python 웹 애플리케이션 Dockerfile**

```
# 기본 이미지 설정
FROM python:3.9

# 작업 디렉토리 설정
WORKDIR /app

# 필요한 파일 복사
COPY . .

# 패키지 설치
RUN pip install -r requirements.txt

# 컨테이너가 열어줄 포트
EXPOSE 5000

# 실행 명령
CMD ["python", "app.py"]
```

이 Dockerfile을 사용하면 Flask와 같은 Python 웹 애플리케이션을 쉽게 컨테이너화할 수 있습니다.

---

### **4. Dockerfile을 이용한 이미지 빌드 및 실행**

### **(1) 이미지 빌드**

```bash
docker build -t my-python-app .
```

- 현재 디렉토리에 있는 `Dockerfile`을 기반으로 `my-python-app`이라는 이름의 이미지를 생성합니다.

### **(2) 컨테이너 실행**

```bash
docker run -p 5000:5000 my-python-app
```

- 컨테이너를 실행하며, 호스트의 5000번 포트를 컨테이너의 5000번 포트와 연결합니다.

---

### **5. Best Practices (베스트 프랙티스)**

### ✅ **가벼운 이미지 사용**

- `ubuntu:latest` 같은 무거운 이미지 대신 `alpine` 기반의 이미지를 사용하는 것이 좋습니다.
    
    ```
    FROM python:3.9-alpine
    ```
    
- 불필요한 패키지를 줄여 컨테이너 크기를 최소화할 수 있습니다.

### ✅ **레이어 최소화**

- `RUN` 명령을 한 줄로 합쳐서 불필요한 레이어 생성을 방지합니다.
    
    ```
    RUN apt-get update && apt-get install -y python3
    ```
    

### ✅ **캐시 활용**

- 자주 변경되는 파일은 Dockerfile 아래쪽에 배치하여 빌드 캐시를 활용하는 것이 좋습니다.
    
    ```
    COPY requirements.txt .
    RUN pip install -r requirements.txt
    COPY . .
    ```
    

### ✅ **멀티스테이지 빌드 사용**

- `multi-stage build`를 활용하여 빌드 과정에서 불필요한 파일을 줄일 수 있습니다.
    
    ```
    FROM node:18 AS builder
    WORKDIR /app
    COPY . .
    RUN npm install && npm run build
    
    FROM nginx:alpine
    COPY --from=builder /app/dist /usr/share/nginx/html
    ```
    

---

### **6. Dockerfile 활용 예제**

### **(1) Node.js 애플리케이션 Dockerfile**

```
FROM node:18

WORKDIR /app

COPY package.json ./
RUN npm install

COPY . .

EXPOSE 3000

CMD ["node", "server.js"]
```

### **(2) Java 애플리케이션 Dockerfile**

```
FROM openjdk:17-jdk-slim

WORKDIR /app

COPY target/myapp.jar .

EXPOSE 8080

CMD ["java", "-jar", "myapp.jar"]
```