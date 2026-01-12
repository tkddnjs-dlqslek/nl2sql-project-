# 🚀 NL2SQL-Financial-Query-Engine
> Gemini 2.5 기반 금융 데이터 자연어 질의 엔진 (Daqube QUVI 프로세스 벤치마킹)

본 프로젝트는 금융 데이터를 대상으로 사용자의 자연어 질문을 이해하고, 이를 정교한 SQL 쿼리로 변환하여 실시간 데이터를 조회하는 **NL2SQL(Natural Language to SQL)** 엔진입니다. 단순한 텍스트 변환을 넘어 시맨틱 레이어를 통한 코드 매핑 및 동적 JOIN 로직을 포함합니다.

## 📌 주요 특징
- **Gemini 2.5 Flash lite 활용**: 사용자의 의도를 분석하여 중간 표현 구조인 SMQ(Semantic Meta Query) 생성.
- **시맨틱 매핑 (Seeds)**: 한글 비즈니스 용어(예: '정기예금')를 실제 DB 코드(예: '0102001') 및 영문 컬럼명으로 자동 치환.
- **동적 JOIN 엔진**: 질문의 맥락에 따라 `customers`와 `transactions` 테이블을 자동으로 결합하여 최적의 쿼리 생성.
- **날짜 도메인 처리**: '작년', '2025' 등 모호한 시간 표현을 SQL의 `BETWEEN` 구문으로 자동 변환.
- **실제와 유사한 가상 데이터**: `Faker` 라이브러리를 통해 생성된 10,000건의 금융 거래 데이터셋 기반.

## 🛠 기술 스택
- **Language**: Python 3.10.11
- **LLM**: Google Gemini 1.5 Flash (via `google-generativeai`)
- **Database**: PostgreSQL (via `psycopg2`)
- **Data Engineering**: `Pandas`, `Faker`
- **IDE**: VS Code (Jupyter Notebook)

## 🏗 시스템 아키텍처


1. **User Query**: "작년에 정기예금에 가입한 VIP 고객은 몇 명이야?"
2. **Semantic Parser**: Gemini가 질문을 분석하여 JSON 형태의 SMQ 생성.
3. **QUVI Processor**: `seeds.json`을 참조하여 한글 명칭을 DB 스키마(영문/코드)로 매핑.
4. **SQL Builder**: PostgreSQL 문법에 맞춰 JOIN 및 날짜 필터가 포함된 SQL 조립.
5. **Executor**: `psycopg2`를 통해 DB 실행 및 결과 반환.

## 📋 실행 방법
1. **DB 구축**: PostgreSQL 설치 후 `make_dummy.ipynb`를 실행하여 3개의 테이블(`customers`, `transactions`, `loans`)과 데이터를 적재합니다.
2. **환경 변수 설정**: 프로젝트 루트 폴더에 `.env` 파일을 생성하고 API 키와 DB 정보를 입력합니다.
3. **엔진 실행**: `engine_test.ipynb`의 모든 셀을 실행한 뒤, 자연어로 질문을 던져 결과를 확인합니다.
