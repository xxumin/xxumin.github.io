## /var/lib/postgresql/data/base
  - postgres 데이터베이스 클러스터 내의 모든 데이터 파일이 저장되는 디렉토리(테이블, 인덱스, 시퀀스 등 객체)
  - 서브 디렉토리명은 Object ID 값으로 결정됨
  
## postgresql.conf
  * \#으로 되어 있는 값은 디폴트 값
    * data_directory: 데이터 디렉토리 설정
    * hba_file: 클라이언트 접속 인증관련 설정
    * max_connection: 서버에 대한 최대 동시 연결 개수
    * shared_buffers: 서버에서 사용하는 공유 메모리 값을 설정. 데이터베이스에 사용할 메모리의 1/4 정도 할당

```sql
select oid, typename from pg_type
where oid = :oid_number
```

```sql
select oid, relname from pg_class
where relname = :table_name
```
---
## 백업과 복원

* [pg_dump](https://www.postgresql.org/docs/current/app-pgdump.html)
* [pg_restore](https://www.postgresql.org/docs/current/app-pgrestore.html)

```bash
PGPASSWORD=${비밀번호}
pg_dump --no-privileges --no-owner --verbose
        -E UTF-8	# 인코딩
        -Z 9		# 압축율
        -h ${호스트 IP} -d ${데이터베이스명} -U ${계정}
        -n ${덤프할 스키마}
        -Fd      # 덤프 파일 포맷 (Fd = 디렉토리, Fp = SQL 스크립트:디폴트)
        -j 20    # parallel jobs(병렬 작업, Fd 옵션일때만 사용가능)
        -f ./${덤프파일이름}
```

```bash
PGPASSWORD=${비밀번호}
pg_restore  --disable-triggers --verbose
            -h ${호스트 IP} -d ${데이터베이스명} -U ${계정}
            -c			# drop and recreate
            -Fd			# 덤프 파일 포맷
            -j 20 		# parallel jobs(병렬 작업, Fd 옵션일때만 사용가능)
            ./${덤프파일이름}
```
---
# 권한 부여
```sql
grant select on schema $schema_name to $user;
grant select on all tables in schema $schema_name to $user;
alter default privileges in schema $schema_name grant select on tables to $user;
```

# 권한 확인
```sql
select *
from information_schema.role_table_grants
where grantee = $user;
```
# 유저 변경
```sql
set role $user;
```
