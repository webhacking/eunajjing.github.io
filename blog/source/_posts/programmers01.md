---
title: 어디가 틀린 거죠 아조씨 코딩 무지랭이는 처음인가요
date: 2019-05-20 23:04:18
tags:
categories:
- 개발공부
- 알고리즘 난항일지
---

# 프로그래머스 - K번째 수

###### 문제 설명

배열 array의 i번째 숫자부터 j번째 숫자까지 자르고 정렬했을 때, k번째에 있는 수를 구하려 합니다.

예를 들어 array가 [1, 5, 2, 6, 3, 7, 4], i = 2, j = 5, k = 3이라면

1. array의 2번째부터 5번째까지 자르면 [5, 2, 6, 3]입니다.
2. 1에서 나온 배열을 정렬하면 [2, 3, 5, 6]입니다.
3. 2에서 나온 배열의 3번째 숫자는 5입니다.

배열 array, [i, j, k]를 원소로 가진 2차원 배열 commands가 매개변수로 주어질 때, commands의 모든 원소에 대해 앞서 설명한 연산을 적용했을 때 나온 결과를 배열에 담아 return 하도록 solution 함수를 작성해주세요.

##### 제한사항

- array의 길이는 1 이상 100 이하입니다.
- array의 각 원소는 1 이상 100 이하입니다.
- commands의 길이는 1 이상 50 이하입니다.
- commands의 각 원소는 길이가 3입니다.

##### 입출력 예

| array                 | commands                          | return    |
| :-------------------- | :-------------------------------- | :-------- |
| [1, 5, 2, 6, 3, 7, 4] | [[2, 5, 3], [4, 4, 1], [1, 7, 3]] | [5, 6, 3] |

##### 입출력 예 설명

[1, 5, 2, 6, 3, 7, 4]를 2번째부터 5번째까지 자른 후 정렬합니다. [2, 3, 5, 6]의 세 번째 숫자는 5입니다.
[1, 5, 2, 6, 3, 7, 4]를 4번째부터 4번째까지 자른 후 정렬합니다. [6]의 첫 번째 숫자는 6입니다.
[1, 5, 2, 6, 3, 7, 4]를 1번째부터 7번째까지 자릅니다. [1, 2, 3, 4, 5, 6, 7]의 세 번째 숫자는 3입니다.

## 내가 푼 것

```javascript
const solution => (array, commands) => {
    
    let index = 0;
    let answer = [];
    
    while(index < commands.length) {
        let count = 0;
        while (count < commands[index].length) {
            const first = commands[index][count++];
            const second = commands[index][count++];
            const third = commands[index][count++];
            const newArray = array.slice(first-1, second);
            newArray.sort();
            answer.push(newArray[third-1]);
        }
        index++;    
    }

    return answer;
}
```

![](/images/programmers-error.png)

다른 분들 푼 거 구경하니까 체이닝 방식으로 엄청 길게 쓰셨던데, 나는.. 그렇게 쓰면 나도 이해를 못하는 터라 다 일일이 풀어 쓴다.... 그리고 뭐가 문제인지 모르겠다... 현재로서는....