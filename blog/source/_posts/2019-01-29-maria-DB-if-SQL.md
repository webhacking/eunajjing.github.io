---
title: maria DB if SQL
date: 2019-01-29 17:55:18
tags:
categories: [개발공부]
---

## maria DB IF문

mysql의 문법처럼, maria DB 또한 if문을 쓸 수 있다.

그런데 내가 궁금한 건 select절에도 if문을 쓸 수 있느냐 여부였다.

즉 이렇게!

```sql
select .... 
if (칼럼명 = '~', 참일 때 실행, 거짓일 때 실행) "별칭"
```

백문이 불여일실행이라고,

해당 쿼리를 실행해보았다.

```sql
select r.rno, r.isFrom, r.targetTypeCode, r.targetCode,
		r.reasonCode, r.rContent, r.processCode,
		if (r.targetTypeCode='BOARD',
		(select mid from board where bno=r.targetCode),
		(select id from comment where cno=r.targetCode)) "isTo",
		b.title from report r join board b on r.targetCode=b.bno;
```

처음에는 괄호를 안 써서 에러가 났다.

문법 자체가 잘못된 건지 모르겠어서 강사 님께 문의를 드렸는데, 논리에 맞지 않는다고 하셨다.

그런데 웃긴 게 내가 어쩌다가 실행한 sql이 되는 걸 확인을 했고

(문제는 그 저장된 sql이 워크밴치가 응답없음이 뜨면서 날아가서...)

계속 미련을 못 버리다가 괄호를 잘 치니까 되었다!!!!

근데 위의 쿼리는 내가 원하는 값을 가져오질 못했다ㅠㅠㅠㅠ 원하는 값을 가져오려면 프로시저를 써야 했다.

하여튼 문법적으로는 되는 쿼리다!