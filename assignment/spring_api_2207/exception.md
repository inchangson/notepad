# 프로젝트 예외 관련 정리

## 예외 처리 방식
- 교육용 프로젝트이니
- 표준 exception [S]
- custom exception [C]
- 메시지 처리 [M]
- 세 가지 모두 적용

## 예외 케이스 정리

### 예외가 일어날 수 있는 상황
1. DB에 존재하지 것들(ex. DB에 등록되지 않는 사용자) [M]
2. 각 파라미터에서, null로 들어오면 안 되는 값들이 null로 들어오는 경우 [C]
3. 정합성(Integer가 들어와야하는데 String이 들어오는 등의) [S]

### 전체 장치 조회 기능
1. 존재하지 않는 사용자 [M]
2. 잘못된 request(userId) [S]

### 개별 장치 조회 기능
1. 존재하지 않는 사용자 [M]
2. 존재하지 않는 기기 [M]
3. 잘못된 request(userId) [S]
4. 잘못된 request(dvcSeq) [S]
   
### 리소스 수정 기능
1. 존재하지 않는 사용자 [M]
2. 존재하지 않는 기기 [M]
3. 잘못된 request(userId) [S]
4. 잘못된 request(dvcSeq) [S]
5. 잘못된 request(rscGrp) [S]
6. 잘못된 request(value) [S]
7. value와 rscGrp에 대한 정합성 [C]

### 장치 삭제 기능
1. 존재하지 않는 사용자 [M]
2. 존재하지 않는 기기 [M]
3. 잘못된 request(userId) [S]
4. 잘못된 request(dvcSeq) [S]

### null 처리
- @Validated 활용