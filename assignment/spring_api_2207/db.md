# DB 설계

## 제안서 요청 사항 정리
[DB 관련]
- “제휴사 + External / Model ID + Model Name // 나머지(자유 구성)”의 구조
- 나머지의 경우에도 변하는 값과 정적인 값은 분리해야 함.
- 회원 정보는 임의로 생성 가능
- deviceSeq는 PK
- Seq가 들어있는 경우 대개 PK, 즉 historySeq는 Log Table의 PK
- serviceTargetSeq는 청약 table의 PK이지만 임의로 사용자 컬럼으로 관리
- 같은 단어 들어가면 대개 같은 테이블 !
- JSON -> Device Name은 별칭을 의미

## 중간 정리
- (1:N) -> 회원이 관리하는 장치(들)

- (1:N) -> 해당 장치가 가지고 있는 리소스(object)이 있다.

+ 리소스는 각 장치마다 공통된 부분이 있다.
   게시글과 같이 형식이 동일한 게 아니라 아예 내용 또한 동일하다

+ 해당 리소스의 값을 관리하는 테이블이 있어야 한다.

## Questions
1. 의미상 관계는 종속이지만기본키 또는 복합키로 구성하지 않아도 될 경우엔?즉 부모로부터 상속받은 테이블에일종의 index기능을 하는 sequence가 있는 경우엔부모와 자식 간의 관계가 무조건 non-identifying으로 해도 되지 않은지?복합키로 쓰냐 안 쓰냐와cascade 설정과는 의미 상 다른 것 아닌지?

2. Delete 해야할 놈의 범위?(useNY를 쓰는 범위)모든 테이블?
3. DB 테이블과 클래스 설계는 꼭 일치해야 하는가?일치하지 않아도 될 듯 ?
4. 기능 중, deviceSeq가 이미 pk의 역할을 할 수 있는데 다른 정보들은 왜 같이 넘겨줘야하는지?ex. 장치 제어 API에서 serviceTargetSeq, external, modelId, deviceName… 물론 resource는 같이 넘겨주는 게 낮다고 생각.(안정성, 성능 때문 ? )
5. Table column 이름 설정할 때,ex) device라고 하면,deviceSeq를 device 테이블의 컬럼 값으로 deviceSeq 자체를 넣을지,orseq라고만 넣고, 다른 테이블에서 호출할 때/ 외부에서 접근할 때만 명시적으로 deviceSeq라고 할지?https://stackoverflow.com/questions/1318272/mysql-naming-conventions-should-field-name-include-the-table-name-> 해당 페이지에 따르면, id, name과 같이 타 테이블과 겹치는 경우엔 붙이고, 아닐 경우엔 떼도 된다고 설명..
6. Naming convention : user_id or userIdjson data 보면 camel case인데 db엔 따로 적용해도 되는지?그랬을 때의 disadvantage?

