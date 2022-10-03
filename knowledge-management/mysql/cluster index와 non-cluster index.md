
# cluster index #anki
- MySQL 서버에서 테이블 레코드를 비슷한 것(프라이머리 키를 기준으로)끼리 묶어서 저장하는 형태로 구현된다.
-   MySQL에서는 InnoDB 스토리지 엔진에서만 지원한다.
-   즉, 프라이머리 키 값에 의해 물리적인 레코드 저장 위치가 결정된다. (데이터 페이지를 건든다)
    -   즉 pk 변경은 신중해야 한다.
-   왜 필요한가?
    -   비슷한 값들을 동시에 조회하는 경우가 많기에 묶어서 조회하면 효율적임.

![[Pasted image 20221003163535.png]]

# non clustered index #anki
-   지정된 컬럼에 대해서 물리적으로 데이터를 재배열하지 않는다.
-   2 단계로 인덱스 페이지를 구성하여 이를 가능케 한다.
	-   중간 단계는 데이터 페이지에 접근할 레퍼런스(RollNo)를 가지고 있다.
	-   최상위 단계는 중간 단계 인덱스 페이지에 접근할 페이지 아이디를 가지고 있다.
	-   즉 레퍼런스만 들고 있기에 물리적인 데이터 재배열을 하지 않는다는 이야기
-   한 테이블 당 여러 non-clustered index를 가질 수 있다.

![[Pasted image 20221003163559.png]]

ref : [](https://velog.io/@gillog/SQL-Clustered-Index-Non-Clustered-Index)[https://velog.io/@gillog/SQL-Clustered-Index-Non-Clustered-Index](https://velog.io/@gillog/SQL-Clustered-Index-Non-Clustered-Index)