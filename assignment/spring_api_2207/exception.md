# 프로젝트 예외 관련 정리

## 예외 케이스 정리

### 예외가 일어날 수 있는 상황
1. DB에 존재하지 것들(ex. DB에 등록되지 않는 사용자)
2. 각 파라미터에서, null로 들어오면 안 되는 값들이 null로 들어오는 경우
3. 정합성(Integer가 들어와야하는데 String이 들어오는 등의)

### 전체 장치 조회 기능
1. 존재하지 않는 사용자
2. 잘못된 request(userId)

### 개별 장치 조회 기능
1. 존재하지 않는 사용자
2. 존재하지 않는 기기
3. 잘못된 request(userId)
4. 잘못된 request(dvcSeq)
   
### 리소스 수정 기능
1. 존재하지 않는 사용자
2. 존재하지 않는 기기
3. 잘못된 request(userId)
4. 잘못된 request(dvcSeq)
5. 잘못된 request(rscGrp)
6. 잘못된 request(value)
7. value와 rscGrp에 대한 정합성

### 장치 삭제 기능
1. 존재하지 않는 사용자
2. 존재하지 않는 기기
3. 잘못된 request(userId)
4. 잘못된 request(dvcSeq)