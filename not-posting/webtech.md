[원본](https://www.notion.so/DevFest-WebTech-CodeLab-f4459c73771b4c3f9be59dbd6e068f94)

- classes
  - Class는 Function declaration 처럼 hoisted 되지 않음
    - new 생성자가 하나의 객체를 만드는 것이 아니라 연결된 무언가를 만드는 것

```javascript
class nameOfClass extends existingClass {
  constructor() {
	  // ...
  }

	someFunction() {
		// ...
	}

	get someVariable() {
		// ...
	}

	set someVariable(variable) {
		// ...
	}

	static someStaticFunction() {
		// ...
  }
}
```

```javascript
class GDGEvent {
    	constructor(eventName, eventDate) {
        this.eventName = eventName
        this.eventDate = eventDate
      }
    
    	updateEvent(eventName, eventDate) {
    		// ...
      }
}
    
class GDGWebTechEvent extends GDGEvent {
      constructor(eventName, eventDate, focusingTopic) {
        super(eventName, eventDate)
        this.focusingTopic = focusingTopic
    		this.eventPlace = GDGWebTechEvent.defaultEventPlace()
      }
    
      updateEvent(eventName, eventDate, focusingTopic) {
    		// do something with focusingTopic
        super.updateEvent(eventName, eventDate)
      }
    
      get eventInformation() {
        return `${this.eventName} will be held at ${this.eventDate} focusing on ${this.focusingTopic}`
      }
    
      static defaultEventPlace() {
        return 'Google for Startups located in Seoul, Korea'
      }
}
    
const devFestWebTech2019 = new GDGWebTechEvent(
    'DevFest WebTech', '21 Nov 2019', 'web technologies'
)
    console.log(devFestWebTech2019.eventInformation)
    // 'DevFest WebTech will be held at 21 Nov 2019 focusing on web technologies'
```

- Enhanced Object Literals

```
const height = 30
const width = 60
const weight = '3kg'
const color = 'ivory'

const myCat = {
	height,
	width,
	weight,
	color,
	[`computed${height * width}`]: height * width
}
```

```
const cat = 'Mambo'
const age = 8

function simpleTag(strings, catExp, ageExp) {
  const firstString = strings[0]
  const secondString = strings[1]

  return `${firstString}${catExp}${secondString}${ageExp > 7 ? 'old' : 'not old'}`
}

const catString = simpleTag`My cat ${ cat } is ${ age }`
console.log(catString)
```

- Destructuring

```
const [a, , b] = [1,2,3]
const [a, , b] = [1,2,3]
```

- array API

```
// Array-like DOM Nodes를 실제 배열 타입으로 래핑
Array.from(document.querySelectorAll('*'))
Array.of(1, 2, 3) // from과 거의 유사하지만 파라미터 개수가 다수일 때

// 7로 1번째부터 채우기
[0, 0, 0].fill(7, 1) // [0, 7, 7]
```

- includes

```
[1, 2, 3].includes(-1)                   // false
[1, 2, 3].includes(1)                    // true
```

- `**`

```
// 5의 세제곱을 구할때 기존의 방법
Math.pow(5, 3)

// 이제는 operand ** operand 이렇게 사용할 수 있어요
5 ** 3 // 125
```

- `Promise.all`



- `Async function`

```
// 기본적인 사용 방법
    async function getFive() {
    	return 5
    }
    
    getFive().then((value) => {
    	console.log(value)
    })
    
    // or
    
    async function getFive() {
    	return 5
    }
    
    async function waitForFive() {
    	const value = await getFive()
    	console.log(value)
    }
    
    waitForFive()
    
    // Thenable objects를 기다릴때
    async function waitForThenable() {
      const thenable = {
        then: function(resolve, reject) {
          resolve('resolved!')
        }
      }
      console.log(await thenable) // resolved!
    }
    waitForThenable()
    
    // API를 호출하는 경우
    
    // Promise로 했을때(ES6에서의 예제와 동일)
    fetch('https://awesome.com/api/cats', { method: 'GET' })
    	.then((response) => response.json())
    	.then(({status, data}) => {
    		// ...
    	})
    
    // Async function과 await을 사용했을때
    async function apiHandler() {
    	const response = await fetch('https://awesome.com/api/cats', { method: 'GET' })
    	const { data } = await response.json()
    	console.log(data)
    }
    apiHandler()
```

- Object entries, values

```
// Object.entries()
const obj = { eventName: 'DevFest WebTech', eventPlace: 'Google for Startups' }
console.log(Object.entries(obj))
// [ ['eventName', 'DevFest WebTech'], ['eventPlace', 'Google for Startups'] ]

// Object.values()
const obj = { foo: 'foo', bar: [100, 200], baz: 55 }
console.log(Object.values(obj))
// ['foo', [100, 200], 55 ]

const myStr = 'Lufthansa'
console.log(Object.values(myStr))
// ["L", "u", "f", "t", "h", "a", "n", "s", "a"]
```





- Array flat & flatMap

```
// Array flat
let arr = ['a', 'b', ['c', 'd']]
let flattened = arr.flat()
console.log(flattened)    // ["a", "b", "c", "d"]

// Array flatMap
const arr = [4.25, 19.99, 25.5]
console.log(arr.map((value) => [Math.round(value)]))
// [[4], [20], [26]]
console.log(arr.flatMap((value) => [Math.round(value)]))
// [4, 20, 26]
```

- Object fromEntries

```
const myArray = [['one', 1], ['two', 2], ['three', 3]]
const obj = Object.fromEntries(myArray)

console.log(obj)    // {one: 1, two: 2, three: 3}
```







### Observable

우리가 애플리케이션을 만들때 데이터는 API에서도 오고, 여러 DOM tag에 달린 이벤트 리스너에서도 오겠죠? 하지만 언제 올지는 아무도 모르죠. 그래서 우리는 이벤트 리스너를 Browser에 `등록` 을 해서 어떤 이벤트가 발생하는지 Browser가 대신 우리가 필요한 작업(콜백 함수)을 실행해 주죠. API도 마찬가지에요. 언제 올지 모르는 비동기성 호출을 도착했을때 적절하게 처리하기 위해 Promise로 API를 핸들링 했어요.

RxJS는 이런 모든 종류의 데이터의 흐름을 표현해주는 객체랍니다. 정확히는 `시간을 축으로 연속적인 데이터를 저장하고 있는 객체` 입니다. 우리가 input tag에 awesome이라고 타이핑을 하면 a → aw → awe → awes → aweso → awesom → awesome 순으로 데이터가 넘어옵니다. API들도 호출한 순서에 따라 컨트롤해야 하는 필요성이 있죠? Observable은 이렇게 시간순으로 오는 데이터를 의미한다고 이해하시면 됩니다.

```javascript
const inputStream = fromEvent(searchInput, 'input')
.pipe(
    // 물이 흐른다
    map((event) => event.target.value),
    debounceTime(1000),
    distinctUntilChanged(),
    // 같은 것이 들어왔다면
    // 바깥으로 물을 쳐낸다
    // 이 아래로 흐르는 물은 다름
    tap(() => recipeList.innerHTML = '<h3>Loading...</h3>'),
    // 데이터와 무관한 활동을 해야할 때
    switchMap((inputValue) =>
    // 옵져버블 안에 있는 값을 꺼내야 할 경우
    // 중첩 파이프라인의 값을 가져옴
    // 팬딩된 요청에 대해서는 무시하며
    // 파이프라인 두 개를 합치는 역할
    
    // 유저 입장에서는 첫 요청보단 두 번째 요청을 먼저 실행되길 원하니까
    
    // 프로미스를 리턴하지 않음
    	ajax(`${apiEndPoint}?f=${inputValue}`, { method: 'GET' })
        .pipe(
        // 새로운 파이프 라인 시작
          map(({ response }) => response ? response.meals : []),
        ),
    ),
  )
  )
```

함수들은 rxjs/operator 안에 있다

### Operator

가장 중요한 Observable을 이해했으니 이건 좀 더 쉽습니다. 이렇게 시간순으로 연속적으로 들어오는 데이터를 우리는 그대로 사용하기도 하지만 계산을 하거나 log를 남기기도 하죠. 이렇게 데이터의 흐름 사이 사이에 어떤 특정한 `작업` 을 해야하는데, 이 작업을 해줄 수 있게 해주는 녀석이 바로 Operator 에요(Operator는 Observable을 만들어내기도 합니다). 좀 더 정확히는, 이 Operator는 Observable을 받아 어떤 작업을 하고 새로운 Observable을 뒤로 넘깁니다. 그럼 다음 Operator가 받겠죠!

### Observer

이렇게 Observable과 Operator로 데이터가 어떻게 흘러서 어떻게 변하는지를 표현했으니, 이 데이터를 결국엔 어딘가에서 꺼내서 활용을 해야겠죠? 그 활용을 하는 녀석이 바로 Observer에요. 그래서 관찰자인거죠! 뒤에 코드로 보면 더 쉽게 이해가 가겠지만, 더 정확히 설명하면 Observer는 next, error, complete 함수를 프로퍼티로 갖는 객체를 의미해요. 이 객체를 `observableInstance.subscribe()` 이렇게 subscribe 함수 안에 넣어주면 데이터를 꺼내오기 시작해요.

### Subscription

Observer가 데이터를 `observableInstance.subscribe()` 로 꺼내오기 시작했어요. 그런데 이렇게 언제 올지 모르는 데이터를 꺼내온다는 것은 RxJS가 Browser 어딘가에 이벤트 핸들러를 달아놨다는 것을 의미하겠죠? 즉, 메모리 자원이 사용되었다는 건데, 더 이상 데이터를 활용하지 않아야 하는 순간이 오면(페이지를 이동하는 등) 사용한 메모리 자원을 다시 반환해야 해요. 그럴 때를 위해 Observable 인스턴스의 subscribe 함수는 subscription 객체를 반환해요. 이 객체를 이용해 `subscription.unsubscribe()` 와 같이 사용한 메모리 자원을 해제할 수 있어요.



```
inputStream.subscribe({
  next: (meals) => {
  // 받고 나서 할 일
    recipeList.innerHTML = meals
      .map((meal) => new Meal(meal))
      .map((mealInstance) => mealInstance.renderToString())
      .join('')
  },
  error: () => {
    recipeList.innerHTML = '<h3>An error has occurred when fetching data...</h3>'
  },
})
```



