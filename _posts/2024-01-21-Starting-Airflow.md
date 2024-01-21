## Apache Airflow
  - 배치 태스크에 중심을 둔 워크플로우 매니지먼트(batch-oriented framework)
  - 파이썬 프레임워크를 사용해 쉽게 데이터 파이프라인을 구축할 수 있게 해준다

## 에어플로우의 주요 구성 요소
  1. 스케줄러: **DAG**를 분석하고 현재 시점에서 DAG의 스케줄이 지난 경우 에어플로우 워커에 DAG의 **태스크**를 예약
  2. 워커: 예약된 태스크를 선택하고 실행
  3. 웹 서버: 스케줄러에서 분석한 DAG를 시각화하고 DAG 실행과 결과를 확인할 수 있는 인터페이스 제공

##### DAG(Directed Acyclic Graph)
  - 방향성 비순환 그래프는 의존성을 가진다
  - **오퍼레이터** 집합에 대한 실행을 **오케스트레이션**하는 역할을 한다

##### Task
  - 작업의 올바른 실행을 보장하기 위한 오퍼레이터의 Wrapper 또는 Manager
  - 태스크는 오퍼레이터의 상태를 관리하고 사용자에게 상태 변경을 표시하는 에어플로우의 내장 컴포턴트

##### Operator
  - 단일 작업을 수행할 수 있는 기능을 제공
  - **BaseOperator**로부터 상속된 PythonOperator, EmailOperator와 같은 다양한 서브클래스를 제공

## 에어플로우가 선택시 고려해야할 점
#### * 에어플로우를 선택하는 이유  
  1. 오픈 소스이기 때문에 특정 벤더에 종속되지 않고 에어플로우를 사용할 수 있다
  2. 파이썬 코드를 이용해 파이프라인을 구현할 수 있기 때문에 파이썬 언어에서 구현할 수 잇는 대부분의 방법을 이용하여 복잡한 커스텀 파이프라인을 만들 수 있다
  3. 파이썬 기반의 에어플로우는 쉽게 확장 가능하고 다양한 시스템과 통합이 가능하다
  4. 수많은 **스케줄링** 기법을 통해 효율적인 파이프라인 구축이 가능하다
  5. **백필(Backfilling)** 기능을 사용하면 과거의 데이터를 손쉽게 재처리할 수 있기 때문에 코드를 변경한 후 재생성이 필요한 데이터 재처리가 가능하다
  6. 웹 인터페이스를 통해 파이프라인 실행 결과를 모니터링할 수 있고 오류를 디버깅하기 위한 편리한 뷰를 제공
    
#### * 에어플로우를 선택하면 안되는 이유
  1. 스트리밍(실시간데이터 처리) 워크플로 및 해당 파이프라인 처리에 적합하지 않을 수 있다
  2. 추가 및 삭제 태스크가 빈번한 동적 파이프라인의 경우에는 적합하지 않을 수 있다
     * 웹 인터페이스는 DAG의 가장 최근 실행 버전에 대한 정의만 표현해주기 때문에, 실행되는 동안 구조가 변경되지 않은 파이프라인에 좀 더 적합하다
  3.파이썬 코드로 DAG를 작성하는 것은 파이프라인 규모가 커지면 굉장히 복잡해질 수 있다
---

## 에어플로우 실행하기
#### Step 1
~~~bash
mkdir -p ~/vol_airflow/dags
~~~

#### Step 2
파이썬을 이용하여 DAG를 정의한다
~~~bash
vi test_dag.py
~~~

#### Step 3
~~~bash
docker run -it -p 18080:8080 \
-v ~/vol_airflow/dags/test_dag.py:/opt/airflow/dags/test_dag.py \
--entrypoint=/bin/sh \
--name airflow \
apache/airflow:2.0.0-python3.8 \
-c '( \
    airflow db init &&
    airflow users create \
    --username admin \
    --password admin \
    --firstname Anonymous \
    --lastname Admin \
    --role Admin \
    --email admin@example.org \
); \
airflow webserver & \
airflow scheduler \
'
~~~

#### Step 4
  * 인터넷 브라우저에 http://localhost:18080 로 접속 후 로그인하여 웹 인터페이스를 확인
