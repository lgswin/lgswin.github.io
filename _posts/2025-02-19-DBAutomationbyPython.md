---
layout: post
title: "Database Automation by python and github action"
date: 2025-2-19
categories: [Database]
tags: [Database automation, python, github action]
---

### 1. **MySQL 데이터베이스 연결**

```python
import pymysql

# Database connection details
DB_HOST = 'localhost'
DB_USER = 'root'
DB_PASSWORD = 'rootpassword'
DB_NAME = 'assignment1'
SQL_SCRIPT_PATH = 'schema_changes.sql'  # sql script

# 데이터베이스 연결 시도
connection = pymysql.connect(
    host=DB_HOST,
    user=DB_USER,
    password=DB_PASSWORD,
    database=DB_NAME
)
```

✅ **기능:** MySQL 데이터베이스에 `pymysql`을 사용하여 연결합니다.

### 2. **SQL 스크립트 파일 로드**

```python
with open(SQL_SCRIPT_PATH, 'r') as file:
    sql_script = file.read()
```

✅ **기능:** `schema_changes.sql` 파일을 열어 SQL 명령문을 문자열로 읽어옵니다.

### 3. **SQL 명령 실행**

```python
cursor = connection.cursor()

# SQL 명령문을 ';' 기준으로 분리하여 실행
for statement in sql_script.split(';'):
    if statement.strip():  # 빈 명령문은 제외
        try:
            cursor.execute(statement)
            print(f"Successfully executed: {statement[:50]}...")  # 실행한 명령문의 첫 50자 출력
        except pymysql.MySQLError as e:
            print(f"Error executing statement: {statement[:50]}... \n{e}")
```

✅ **기능:** SQL 파일의 명령어를 하나씩 실행하며, 실행 성공 여부를 출력합니다.

### 4. **변경 사항 커밋 및 연결 종료**

```python
connection.commit()
print("🎉 All SQL statements executed successfully!")
```

✅ **기능:** 모든 SQL 명령 실행 후 변경 사항을 커밋합니다.

```python
finally:
    if 'cursor' in locals():
        cursor.close()
    if 'connection' in locals():
        connection.close()
    print("🔌 Connection closed.")
```

✅ **기능:** 예외 발생 여부와 관계없이 MySQL 연결 및 커서를 종료합니다.

```sql
-- 테이블 생성 예시
CREATE TABLE IF NOT EXISTS users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- 기존 테이블에 컬럼 추가
ALTER TABLE users ADD COLUMN age INT;

-- 새로운 데이터 삽입
INSERT INTO users (name, email, age) VALUES ('Alice', 'alice@example.com', 30);
INSERT INTO users (name, email, age) VALUES ('Bob', 'bob@example.com', 25);
```

✅ **예상 SQL 기능:**

- 테이블 생성 (`CREATE TABLE`)
- 기존 테이블 수정 (`ALTER TABLE`)
- 데이터 삽입 (`INSERT INTO`)

## **📌 SQL 백업 로직**

이 스크립트는 MySQL 데이터베이스를 백업하는 역할을 합니다.

### **1. 백업 파일명 생성 (고유한 이름 유지)**

```python
import os
import subprocess
import datetime

# 데이터베이스 이름과 백업 디렉토리 설정
db_name = "assignment1"  # 백업할 데이터베이스
backup_dir = "./backups"  # 백업 파일을 저장할 로컬 디렉토리

# 현재 날짜와 시간을 기반으로 백업 파일명 생성 (YYYYMMDD_HHMMSS.sql)
timestamp = datetime.datetime.now().strftime("%Y%m%d_%H%M%S")
backup_filename = f"{db_name}_backup_{timestamp}.sql"
b_path = os.path.join(backup_dir, backup_filename)  # 저장할 로컬 경로
```

✅ **기능:**

- `datetime` 모듈을 사용하여 현재 시간을 기반으로 고유한 백업 파일명을 생성합니다.
- 백업 파일을 저장할 `./backups` 폴더를 설정하고 백업 파일명을 `assignment1_backup_YYYYMMDD_HHMMSS.sql` 형식으로 만듭니다.

### **2. 백업 폴더 존재 여부 확인 및 생성**

```python
# 백업 폴더가 없으면 생성
if not os.path.exists(backup_dir):
    os.makedirs(backup_dir)
```

✅ **기능:**

- `os.path.exists()`를 사용하여 `./backups` 폴더가 존재하는지 확인하고, 없다면 `os.makedirs(backup_dir)`를 실행하여 자동으로 폴더를 생성합니다.

---

### **3. `mysqldump`를 사용하여 데이터베이스 백업**

```python
try:
    # MySQL 백업 실행
    print(f"🚀 Creating backup: {backup_filename}")
    backup_command = f"mysqldump -u root -p'rootpassword' {db_name} > {b_path}"
    subprocess.run(backup_command, shell=True, check=True)

    print(f"✅ Backup successfully! File saved at: {b_path}")

except subprocess.CalledProcessError as e:
    print(f"❌ Error during database backup: {e}")
```

✅ **기능:**

- `mysqldump` 명령어를 `subprocess.run()`을 사용하여 실행합니다.
- MySQL root 계정(`u root -p'rootpassword'`)을 사용하여 `assignment1` 데이터베이스의 백업
- 생성된 백업 파일은 `./backups/assignment1_backup_YYYYMMDD_HHMMSS.sql`에 저장됩니다.
- `subprocess.run(..., check=True)`를 사용하여 `mysqldump` 실행 중 오류가 발생하면 예외를 출력

### **4. ZIP 파일에서 SQL 스크립트 추출 및 실행**

```python
with zipfile.ZipFile(PACKAGE_FILE, 'r') as zf:
    for script in zf.namelist():
        print(f"📂 Processing {script}...")
        with zf.open(script) as file:
            sql_script = file.read().decode('utf-8')

            # SQL 스크립트 실행
            statements = sql_script.split(';')
            for statement in statements:
                if statement.strip():  # 빈 명령어 제외
                    try:
                        cursor.execute(statement)
                        connection.commit()
                        print(f"✔ Executed: {statement[:50]}...")  
                    except pymysql.MySQLError as e:
                        print(f"❌ Error executing: {statement[:50]}...\n{e}")
```

✅ **기능:**

- ZIP 파일을 열어 `.sql` 파일을 하나씩 읽음.
- SQL 파일을 문자열로 변환한 후 `;` 기준으로 나누어 실행.
- `commit()`을 호출하여 변경 사항을 저장.
- 실행 성공 여부 및 오류 발생 시 메시지를 출력.

### **5. SQL 파일 실행 함수 (`execute_sql_file()`)**

```python
python
CopyEdit
def execute_sql_file():
    try:
        # 데이터베이스 연결
        connection = mysql.connector.connect(**DB_CONFIG)
        cursor = connection.cursor()

        # SQL 파일 읽기
        with open(SQL_FILE_PATH, "r", encoding="utf-8") as file:
            sql_script = file.read()

        # SQL 문 실행
        for statement in sql_script.strip().split(";"):
            if statement.strip():  # 빈 SQL 문 제외
                cursor.execute(statement)

        # 변경 사항 적용 (commit)
        connection.commit()
        print("✅ SQL script from file executed successfully.")

    except Error as e:
        print(f"❌ Error executing SQL script: {e}")
        if connection:
            connection.rollback()  # 오류 발생 시 롤백

    finally:
        # 연결 종료
        if cursor:
            cursor.close()
        if connection:
            connection.close()
        print("🔌 Database connection closed.")

```

✅ **기능:**

- SQL 파일을 열어 내용을 읽음.
- SQL 문을 `;` 기준으로 나눠서 실행.
- 실행 후 `commit()`을 호출하여 변경 사항을 저장.
- 오류 발생 시 `rollback()`을 수행하여 데이터베이스를 원래 상태로 되돌림.
- 실행이 끝나면 **MySQL 연결 종료**.

---

## 📌 **Database Backup and Performance Monitoring Script**

**MySQL 데이터베이스 백업을 수행하면서 CPU 및 메모리 사용량을 모니터링**하는 기능을 포함

## **🎯 주요 기능**

1. **MySQL 데이터베이스 백업 (`backup_database()`)**
    - `mysqldump` 명령어를 사용하여 `firstdatabase`의 백업을 수행
    - 백업 파일 이름: `backup_YYYYMMDDHHMMSS.sql` (현재 날짜/시간을 포함)
2. **시스템 성능 로그 (`log_system_performance(phase)`)**
    - `psutil` 라이브러리를 사용하여 CPU 및 메모리 사용량을 측정
    - 백업 전후의 성능을 비교
3. **전체 실행 흐름**
    - **초기 성능 측정** (`Initially`)
    - **백업 전 성능 측정** (`Before Backup`)
    - **백업 실행** (`backup_database()`)
    - **백업 후 성능 측정** (`After Backup`)
    - 성능 모니터링 완료 메시지 출력

## **📜 코드**

```python
import os
import datetime
import psutil

def backup_database():
    """ MySQL 데이터베이스 백업 수행 """
    backup_file="backup_"+datetime.datetime.now().strftime("%Y%m%d%H%M%S")+".sql"
    os.system("mysqldump -u root -p firstdatabase > " + backup_file)
    print(f"✅ Database backup completed: {backup_file}")

def log_system_performance(phase):
    """ 현재 시스템의 CPU 및 메모리 사용량을 출력 """
    cpu_usage = psutil.cpu_percent(interval=1)
    memory_info = psutil.virtual_memory()

    print(f"📊 System Performance ({phase}):")
    print(f"  🖥️ CPU Usage: {cpu_usage}%")
    print(f"  🗂️ Memory Usage: {memory_info.percent}%")
    print("="*40)

if __name__ == "__main__":
    print("📚 Course: Database Automation")
    print("🚀 Starting Performance Monitoring...")

    # 초기 시스템 성능 로그
    log_system_performance("Initially")

    # 백업 실행 전후의 성능 비교
    log_system_performance("Before Backup")
    backup_database()
    log_system_performance("After Backup")

    print("✅ Performance Monitoring Completed.")
    print("🎉 Thanks!")
```

---

## 📌 **GitHub Actions CI/CD Pipeline for Azure MySQL Server**

이 GitHub Actions Workflow는 **Azure MySQL Server**에 대한 CI/CD 배포를 자동화하는 구성입니다.

코드를 `main` 브랜치에 푸시하면 **Python 스크립트를 실행하여 MySQL 데이터베이스 변경 사항을 적용**합니다.

## **🎯 주요 기능**

1. **코드 체크아웃 (Checkout Repository)**
    - 최신 코드 가져오기 (`actions/checkout@v2`)
2. **MySQL 클라이언트 설치**
    - `mysql-client`를 설치하여 Azure MySQL 서버와의 연결을 지원
3. **Python 및 필수 패키지 설치**
    - `mysql-connector-python`을 설치하여 Python 스크립트에서 MySQL과 연동
4. **Python 스크립트 실행 (`execute_sql.py`)**
    - Azure MySQL Server의 **환경 변수 (`secrets` 사용)**를 이용하여 SQL 실행

## **📌 GitHub Actions Workflow 설명**

```yaml
name: CI/CD Pipeline

on:
  push:
    branches:
      - main  # main 브랜치에 코드가 푸시될 때 실행

jobs:
  deploy:
    runs-on: ubuntu-latest  # 최신 Ubuntu 환경에서 실행
    steps:
      - name: Checkout code
        uses: actions/checkout@v2  # 최신 코드 가져오기

      - name: Install MySQL client
        run: sudo apt-get update && sudo apt-get install -y mysql-client  
        # MySQL 클라이언트 설치

      - name: Install Python dependencies
        run: |
          sudo apt-get install -y python3 python3-pip  # Python3 및 pip 설치
          pip3 install mysql-connector-python # MySQL과 Python 연결을 위한 패키지 설치

      - name: Deploy to Database using Python
        env:
          DB_HOST: ${{ secrets.DB_HOST }}  # Azure MySQL Server 호스트
          DB_USER: ${{ secrets.DB_ADMIN_USER }}  # MySQL 사용자명
          DB_PASSWORD: ${{ secrets.DB_PASSWORD }}  # MySQL 비밀번호
          DB_NAME: ${{ secrets.DB_NAME }}  # 대상 데이터베이스명
        run: python3 execute_sql.py  # Python 스크립트를 실행하여 MySQL 배포 수행
```

## **🔹 환경 변수 설정 (GitHub Secrets 사용)**

Azure MySQL Server 정보를 GitHub Secrets에 등록하여 보안성을 강화합니다.

| Secret Name | Description |
| --- | --- |
| `DB_HOST` | Azure MySQL Server 호스트 (예: `your-db.mysql.database.azure.com`) |
| `DB_ADMIN_USER` | 데이터베이스 관리자 계정 (예: `admin@your-db`) |
| `DB_PASSWORD` | MySQL 관리자 비밀번호 |
| `DB_NAME` | 대상 데이터베이스 이름 |

📌 **GitHub Secrets 설정 방법**

1. GitHub **Repository** > **Settings** > **Secrets and variables** > **Actions** 로 이동
2. `New repository secret` 클릭 후 위 **환경 변수(Secrets)** 추가

---