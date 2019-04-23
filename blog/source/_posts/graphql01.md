---
title: GraphQL
date: 2019-01-12 15:43:18
tags:
categories:
- 개발공부
- GraphQL
---

## [GraphQL](https://graphql.org/)

- 페이스북에서 만든 어플리케이션 레이어 쿼리 언어
  - API를 위한 쿼리 언어
- 기존에 백엔드와 프론트엔드를 통신하기 위해 `REST API` 를 썼던 것처럼, `REST API` 와 다른 Sever API 개발 방법론
- 특정한 데이터베이스, 스토리지 엔진과 관계가 없고 기존 코드와 데이터에 의해 대체됨

### 일반적인 `REST API` 는

![](https://cdn-images-1.medium.com/max/1600/1*zIHqyyQZ9R_SjoFbFUdu0A.jpeg)

- 여러 URL에서 데이터를 받아야와야 할 경우

- 예시

  `GET /accounts`

  ```javascript
  {
    "accounts": [
      {
        "id": "1",
        "username": "velopert",
        "email": "public.velopert@gmail.com",
        "friends": [
          "2",
          "3"
        ],
        "first_name": "Minjun",
        "last_name": "Kim"
      },
      {
        "id": "2",
        "username": "jn4kim",
        "email": "jn4kim@gmail.com",
        "friends": [
          "1",
          "3"
        ],
        "first_name": "Jayna",
        "last_name": "Kim"
      },
      {
        "id": "3",
        "username": "abet",
        "email": "abet@gmail.com",
        "friends": [
          "2"
        ],
        "first_name": "Abet",
        "last_name": "Bane"
      }
    ]
  }
  ```

  만약 이 데이터에서 특정 `id`의 `friends` 의 이름을 가져온다면 

  요청 url은 `GET /accounts/1/?include_friend_details=username,first_name` 이다.

  혹은 `GET /accounts_with_friend_details/1`

### `REST API` 의 문제는

- 어플리케이션 규모가 커지면 수많은 `endPoint` 들이 생성된다.
  - `endPoint` : 기능에서 필요로하는 정보들에 호응할 수 있는 포인트
- `endPoint`와 그에 걸맞는 데이터를 반환해주는 API를 개발해야 했음

### `GraphQL` 의 등장

![](https://cdn-images-1.medium.com/max/1600/1*PHxksCXy0Wh5ryANEZkfDQ.jpeg)

- 요청하는 리소스마다 `endPoint`  가 다른 `REST API` 와는 달리 

- 클라이언트측에서 쿼리를 만들어서 서버로 보내면 우리가 원하는대로 결과를 반환

  ![](https://blog.sapzil.org/public/img/2015-08-31-graphql-example.jpg)

  `query`

  ```javascript
  query {
      account(id: "1") {
          username
          email
          firstName
          lastName
          friends {
              firstName
              username
          }
      }
  }
  ```

  `result`

  ```javascript
  {
    "data": {
      "account": {
        "username": "velopert",
        "email": "public.velopert@gmail.com",
        "firstName": "Minjun",
        "lastName": "Kim",
        "friends": [
          {
            "firstName": "Jayna",
            "username": "jn4kim"
          },
          {
            "firstName": "Abet",
            "username": "abet"
          }
        ]
      }
    }
  }
  ```

- 필요한 정보를 쿼리로 만들어서 서버에 전달하면 서버가 알아서 주어진 틀대로 데이터를 보여준다

- 쿼리를 통해 **딱 필요한 데이터만 `fetching`하기 때문에 overfetch, underfetch를 할 걱정이 없다**

### GraphQL 스키마 언어

```javascript
type Character {
// 객체 타입
// ! = non-nullable
  name: String!
  // 필드명 : String 타입이며 null일 수 없음
  appearsIn: [Episode]!
  // 필드명 : Episode 객체의 배열이며 null이 아님

  // 만약 필드명 : [Episode!]라면
  // 리스트 자체는 null일 수 있어도 null을 가질 수 없음
}
```

#### 인자

```javascript
type Starship {
  length(unit: LengthUnit = METER): Float
  // length라는 필드는 하나의 인자(unit)를 지닌다.
  // 만약 unit 인자를 전달받지 못한다면
  // default 값은 'METER'이다.
  // 자료형은 Float이다.
}
```

#### 스칼라 타입

```javascript
// 쿼리
{
  hero {
    name
    appearsIn
  }
}

// 결과
{
  "data": {
    "hero": {
      "name": "R2-D2",
      "appearsIn": [
        "NEWHOPE",
        "EMPIRE",
        "JEDI"
      ]
    }
  }
}
```

- `name` 과 `appearIn` 은 하위 쿼리가 없으므로 스칼라 타입이다.

##### 타입 종류

- `Int`: 부호가 있는 32비트 정수.

- `Float`: 부호가 있는 부동소수점 값.

- `String`: UTF-8 문자열.

- `Boolean`: `true` 또는 `false`.

- `ID`: ID 스칼라 타입은 객체를 다시 요청하거나 캐시의 키로써 자주 사용되는 고유 식별자를 나타냅니다. ID 타입은 String 과 같은 방법으로 직렬화된다.

- 열거형 타입(`Enums`)

  - 특정 값들로 제한되는 특별한 종류의 스칼라
  - 타입의 인자가 허용된 값 중 하나임을 검증

  ```javascript
  enum Episode {
    NEWHOPE
    EMPIRE
    JEDI
  }
  // Episode 타입이라면 반드시 위의 셋 중 하나
  ```

#### 커스텀 타입

```javascript
scalar Date
```

*이거는 사실 이해를 못하겠다. 믿는다 미래의 나...!*

#### 특수 타입

- 모든 GraphQL 서비스는 `query` 타입을 가진다.
- `mutation` 타입은 가질 수도 있고 가지지 않을 수도 있다.

```javascript
query {
  hero {
    name
  }
  droid(id: "2000") {
    name
  }
}

// 이 쿼리를 실행시키려면 우선 선행적으로
// hero와 droid 필드가 있는 Query 타입이 필요

type 타입명 {
  hero(episode: Episode): Character
  // 객체 타입
  droid(id: ID!): Droid
}
```

#### 다형성 지원

- 인터페이스로 공통 필드 정의 가능

  ```javascript
  interface Character {
    id: ID!
    name: String!
    friends: [Character]
    appearsIn: [Episode]!
  }
    
  // Character를 구현한 모든 타입은 이러한 인자와 리턴 타입을 가진 정확한 필드를 가져야 함
    
  type Human implements Character {
    id: ID!
    name: String!
    friends: [Character]
    appearsIn: [Episode]!
    starships: [Starship]
    totalCredits: Int
  }
  
  type Droid implements Character {
    id: ID!
    name: String!
    friends: [Character]
    appearsIn: [Episode]!
    primaryFunction: String
  }
      
  // 추가 필드 구현도 가능
  ```

- union 타입으로 여러 타입을 하나로 묶을 수 있음

  ```javascript
  union SearchResult = Human | Droid | Starship
  ```

  - 유니온 타입의 멤버는 구체적인 객체 타입이어야 한다.

    - 즉 `Human`, `Droid`, `Starship` 은 객체 타입이다.

  - 인터페이스, 유니온 타입에서 다른 유니온 타입 사용이 불가하다

    - 만약 유니온 타입 반환 필드를 쓰고자 하면 프래그먼트를 사용해야 한다.

    ```javascript
    // search가 유니온 타입을 반환하는 필드를 쿼리한다는 가정
    
    {
      search(text: "an") {
        ... on Human {
          name
          height
        }
        ... on Droid {
          name
          primaryFunction
        }
        ... on Starship {
          name
          length
        }
      }
    }
    ```

#### 입력 타입

```javascript
input ReviewInput {
  stars: Int!
  commentary: String
}
```

- 뮤테이션에서 특히 유용하다.

```javascript
mutation CreateReviewForEpisode($ep: Episode!, $review: ReviewInput!) {
  createReview(episode: $ep, review: $review) {
    stars
    commentary
  }
}

{
  "ep": "JEDI",
  "review": {
    "stars": 5,
    "commentary": "This is a great movie!"
  }
}
```

- 입력 객체 타입을 참조할 수 있지만, 입력 및 룰력 타입을 스키마에 혼합할 수 없다.
- 필드에 인자를 가질 수 없다.

### `GraphQL API` 의 구성

- GraphQL은 스키마가 미리 정의되어 있는 강타입 언어

- 주석은 `#`  으로 시작

- GraphQL 문서에 쿼리 하나만 있는 경우가 아니면 **명시적으로 쿼리임을 나타내야 한다.**

  ```javascript
  query sampleQuery {
  // 쿼리임을 명시적으로 선언
  	post
  	users
  }
  ```

#### selection set

```javascript
// 쿼리
{
	id
	text
}

// 결과
{
	"id": 42,
	"text": "Hello, world!"
}
```

#### 필드에는 인자를 넘길 수 있다

```javascript
{
	pictureURL(width: 50, height: 50)
}

// 혹은

{
  human(id: "1000") {
    name
    height
  }
}
```

#### 필드 이름은 별명(alias) 지정이 가능

같은 필드를 다른 인자로 가져올 때도 사용

```javascript
{
	fullName: name
	smallPic: profilePic(size: 64)
	bigPic: profilePic(size: 1024)
}
```

#### 필드가 객체나 list 타입이라면 하위 객체의 selection set를 **반드시** 명기

```javascript
{
	id
	text
	author {
		name
		pictureURL(width: 50, height: 50)
		posts {
			id
			text
		}
	}
}
```

- GraphQL 쿼리의 가장 바깥 selection set는 ‘쿼리 루트’ 객체로부터 시작하도록 정해져 있다.

  ```javascript
  {
  	me {
  		name
  	}
  	post(id: "42") {
  		title
  	}
  }
  ```

#### 프래그먼트(fragment)

- 같은 selection set이 한 쿼리 안에서 중복될 수 있다.
- 글쓴이의 프로필과 댓글 작성자의 프로필이 같은 컴포넌트라면 같은 필드가 필요하기 때문
- 중복을 제거하는데 프래그먼트를 사용할 수 있다.

```javascript
query sampleQuery {
	post(id: "4") {
		id
		text
		author {
			...basicUserInfo
		}
	}
	users {
		...basicUserInfo
	}
}

// User 타입에 대한 프래그먼트임을 명시
fragment basicUserInfo on User {
	name
	pictureURL(width: 50, height: 50)
}
```

```javascript
query samplePolymorphicQuery {
	timeline {
		data {
            // data가 Post 리스트일 때
			// 인라인 프래그먼트를 이렇게 지정
			... on Post {
				title
			}

			// 인라인 프래그먼트가 아닐 때는 이렇게 지정
			...basicUserInfo
		}
	}
}
```

#### 메타 필드

- 리턴될 타입을 모르는 상황이 발생하면 클라이언트에서 데이터 처리를 어떻게 할 것인지 결정해야 함
- `__typename` 을 사용하면 그 시점의 객체 타입의 이름이 리턴된다.

```javascript
{
  search(text: "an") {
    __typename
    // 일단 어떤 타입이든 어떤 객체 타입인지 데이터가 출력됨
    ... on Human {
      // 만약 객체 타입이 Human이라면
      name
    }
    ... on Droid {
      name
    }
    ... on Starship {
      name
    }
  }
}

// 결과
{
  "data": {
    "search": [
      {
        "__typename": "Human",
        "name": "Han Solo"
      },
      {
        "__typename": "Human",
        "name": "Leia Organa"
      },
      {
        "__typename": "Starship",
        "name": "TIE Advanced x1"
      }
    ]
  }
```

#### 변수 지정도 가능

```javascript
query HeroComparison($first: Int = 3) {
    // 쿼리나 뮤테이션에 선언된 변수는
    // 프래그먼트에 접근이 가능하다.
  leftComparison: hero(episode: EMPIRE) {
    ...comparisonFields
  }
  rightComparison: hero(episode: JEDI) {
    ...comparisonFields
  }
}

fragment comparisonFields on Character {
  name
  friendsConnection(first: $first) {
      // 이 경우 프래그먼트에 3이 접근한다.
    totalCount
    edges {
      node {
        name
      }
    }
  }
}
```

##### 변수의 정의 `($episode: Episode)`

- 정적타입 언어의 함수에 대한 인자 정의
- `$` 접두사가 붙은 모든 변수를 나열하고 그 뒤에 타입이 온다
- 선언된 변수는 스칼라, 열거형, input object type이어야 한다.
- `Episode` 타입 옆에 `!`가 없다면 옵셔널이다.

#### 지시어

```javascript
{
  "episode": "JEDI",
  "withFriends": false 
}

query Hero($episode: Episode, $withFriends: Boolean!) {
  hero(episode: $episode) {
    name
    friends @include(if: $withFriends) {
      name
    }
  }
}

// true와 false의 결과가 다름을 확인할 수 있다
```

- `@include(if: Boolean)` : 참일 때만 필드가 결과에 포함된다.
- `@skip(if: Boolean)` 인자가 `true` 이면 이 필드를 건너뛴다.

#### mutation

- 데이터의 읽기 외에 쓰기(변형)도 지원함
- 쿼리는 필드를 순서 없이 평가
- **mutation의 필드는 항상 순서대로 평가**, 즉 하나의 요청에서 두 개의 뮤테이션 건을 보낸다면 첫 번째 뮤테이션은 두 번째 요청 전에 완료가 보장된다.

```javascript
mutation sampleMutation {
	setName(name: "Zuck") {
		newName
	}
}
```

### 참고 레퍼런스

- [graphql-kr](https://graphql-kr.github.io/learn/thinking-in-graphs/)

- [매력적인 쿼리 언어, GraphQL](https://dev4us.tistory.com/17)
- [GraphQL 강좌 1편: GraphQL이 무엇인가?](https://velopert.com/2318)

- [한 단계씩 배워보는 GraphQL](https://engineering.huiseoul.com/%ED%95%9C-%EB%8B%A8%EA%B3%84%EC%94%A9-%EB%B0%B0%EC%9B%8C%EB%B3%B4%EB%8A%94-graphql-421ed6215008)

- [가장 현대적인 웹을 만들자](https://medium.com/@kiyeopyang/%EA%B0%80%EC%9E%A5-%ED%98%84%EB%8C%80%EC%A0%81%EC%9D%B8-%EC%9B%B9%EC%9D%84-%EB%A7%8C%EB%93%A4%EC%9E%90-3%ED%8E%B8-graphql-cb69ce1a64b5)

- [GraphQL 살펴보기](https://blog.sapzil.org/2015/09/01/graphql-rfc/)