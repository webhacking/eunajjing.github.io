---
title: notification API를 이용한 웹 알림 구현
date: 2018-10-16 12:43:02
tags:
categories:
- 개발공부
---
# notification API를 이용한 웹 알림 구현

    <!DOCTYPE html>
     <html>
      <head>
       <meta charset="UTF-8">
       <title>Insert title here</title>
      </head>
      <body>
	   <button onclick="notify()">알림 띄우기</button>
	   <script type="text/javascript">
	     window.onload = function() {
		if(window.Notification) {
			Notification.requestPermission();
		} // 온 로드되면 공지 기능을 허용한다.
	     function notify() {
			var notification = new Notification('공지 타이틀', {
				icon: 'https://camo.githubusercontent.com/7710b43d0476b6f6d4b4b2865e35c108f69991f3/68747470733a2f2f7777772e69636f6e66696e6465722e636f6d2f646174612f69636f6e732f6f637469636f6e732f313032342f6d61726b2d6769746875622d3235362e706e67',
				body: 'github 블로그로 이동'
			});

			notification.onclick = function() {
				window.open('http://eunajjing.github.io');
			};
			// 알림 메시지 클릭하면 해당 페이지 새 창이 뜬다.
	      }
       </script>
      </body>
    </html>

원래 커뮤니티 사이트에서 내가 쓴 게시물에 댓글 달면 댓글 달렸다고 알리는 기능? 구현하고 싶어서 검색하다 발견한 소스. 간단하게 잘라서 구현해봤다.

내가 원한 건 이게 아닌데...
