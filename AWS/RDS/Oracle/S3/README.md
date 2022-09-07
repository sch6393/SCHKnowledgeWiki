S3 관련 명령어
===

### [작업 전 S3에 접근하기 위해선 S3 INTEGRATION 추가가 필요](./S3INTEGRATION.md)

<br>

### RDS 디렉토리, 파일 확인
```sql
SELECT * FROM TABLE(rdsadmin.rds_file_util.listdir('DATA_PUMP_DIR')) ORDER BY MTIME;
```

<br>

### S3로부터 파일 다운로드 (S3 ➞ RDS 인스턴스)
```sql
--Download File
SELECT rdsadmin.rdsadmin_s3_tasks.download_from_s3(
      p_bucket_name    =>  's3_bucket_name', 
      p_s3_prefix      =>  'folder_name/file_name.dmp', 
      p_directory_name =>  'DATA_PUMP_DIR') 
   AS TASK_ID FROM DUAL;

--Download Folder
SELECT rdsadmin.rdsadmin_s3_tasks.download_from_s3(
      p_bucket_name    =>  's3_bucket_name', 
      p_s3_prefix      =>  'folder_name/', 
      p_directory_name =>  'DATA_PUMP_DIR') 
   AS TASK_ID FROM DUAL;

--Task ID Log 확인
SELECT text FROM table(rdsadmin.rds_file_util.read_text_file('BDUMP','dbtask-1652167354883-36.log'));
```

<br>

### S3로 파일 업로드 (RDS 인스턴스 ➞ S3)
```sql
--Upload File
SELECT rdsadmin.rdsadmin_s3_tasks.upload_to_s3(
  p_bucket_name    =>  's3_bucket_name',
  p_s3_prefix      =>  'folder_name/file_name.dmp', 
  p_directory_name =>  'DATA_PUMP_DIR') 
AS TASK_ID FROM DUAL;

--Upload Folder
SELECT rdsadmin.rdsadmin_s3_tasks.upload_to_s3(
  p_bucket_name    =>  's3_bucket_name',
  p_s3_prefix      =>  'folder_name/', 
  p_directory_name =>  'DATA_PUMP_DIR') 
AS TASK_ID FROM DUAL;

--Task ID Log 확인
SELECT text FROM table(rdsadmin.rds_file_util.read_text_file('BDUMP','dbtask-1652167354883-36.log'));
```

<br>

### RDS 인스턴스 안에 있는 파일 삭제
```sql
--Delete File
EXEC UTL_FILE.FREMOVE('DATA_PUMP_DIR', 'file_name.dmp');
```

<br>
