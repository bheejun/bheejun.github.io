# WIL(2023-04-03 ~ 2023-04-08)


## 정리할 내용
* Hash탐색 알고리즘


## 해쉬 탐색 알고리즘


발단: 프로그래머스 42576 문제를 풀다가 시간초과에 걸려 문제를 통과하지 못함

문제 설명
---
수많은 마라톤 선수들이 마라톤에 참여하였습니다. 단 한 명의 선수를 제외하고는 모든 선수가 마라톤을 완주하였습니다.

마라톤에 참여한 선수들의 이름이 담긴 배열 participant와 완주한 선수들의 이름이 담긴 배열 completion이 주어질 때, 완주하지 못한 선수의 이름을 return 하도록 solution 함수를 작성해주세요.

제한사항
---
마라톤 경기에 참여한 선수의 수는 1명 이상 100,000명 이하입니다.
completion의 길이는 participant의 길이보다 1 작습니다.
참가자의 이름은 1개 이상 20개 이하의 알파벳 소문자로 이루어져 있습니다.
참가자 중에는 동명이인이 있을 수 있습니다.



## 해결과정


우선 시간 복잡도에 걸려 정답이 되지 못했다는 것은 탐색 알고리즘에 문제가 있었다는 의미!

먼저 작성한 코드의 핵심만 보자면


        List<String> participantList =new ArrayList<>(Arrays.asList(participant)) ;


        for (int i = 0; i < completion.length; i++) {
            if(participantList.contains(completion[i])){
                participantList.remove(completion[i]);

            }
        }

단순히 선형으로 탐색한 것에 불과한 기존 코드. 물론 브루트포스는 강하지만 **개선할 필요를 느꼈다**

먼저 탐색 알고리즘에 대해 검색 해 보았다.

아래는 대표적이고 기본적인 탐색 알고리즘이라 여겨진다.

* 이진탐색 알고리즘
* 브루트 포스
* 해시 탐색


먼저 이 문제는 해시탐색을 적용해야 한다.
1. 이진 탐색 알고리즘은 해당 문제에 적용이 힘들다. 이진 탐색은 정렬된 배열 내에서 위력이 발휘된다.
2. 브루트 포스는 이미 실패했다
3. 해시 탐색은 String 배열에 많이 적용시키는 것 같았다. 또한 검색이 빠르다! 무엇보다 문제의 카테고리가 해시이다.





기본적인 개념은 다음과 같다.

### **해시 탐색법의 개념**

해시 = 잘게 썬다 = 잘게 자른다

* 해시 함수

어떤 값이 주어진 경우, 그 값을 대표하는 숫자를 계산하는 함수

* 해시값: 해시 함수의 계산으로 산출된 값

*장점*

미리 해시 함수를 사용하여 데이터를 저장하는 장소를 정해 두어 검색 시간을 단축

cs적으로는 그렇다는 거고 나는 java에서 제공하는 메소드를 이용하면 됨

대표적으로는 hashmap이 있다.

앞서 설명한 원리에 따라 알고리즘화 하여 메서드를 제공해주는게 hashmap인 것

key와 value를 1:1로 매핑해서 저장하고 검색도 한다.

이 과정에서 key값이 같으면 절대로 안되지만 절대로 그런일이 일어난다.

이를 해결하기 위해 여러가지 방법이 있지만? 

오늘은 그냥 hashmap이 다 해결해 준다.



물론 cs적으로 근본있게 파보고 싶긴한데 갈길이 멀기 때문에... 필요하면 다시 공부할 것

## 그렇다면 hash map은 어떻게 쓰는 것일까?


앞선 설명에 모든것이 들어있다. 

HashMap<String, Integer>로 지정하면 Key는 String 형태, Value는 Integer 형태로 정의하는 것이다.

원한다면 Integer: Integer도 가능함

대표적으로 쓰이는 매서드들중에 대표격인 3가지는

get, put, getOrDefault

* get(key) 메서드: 지정된 key 값에 해당하는 value 값을 반환합니다. 만약 key 값이 존재하지 않으면 null을 반환

* put(key, value) 메서드: 지정된 key 값에 해당하는 value 값을 HashMap에 추가합니다. 만약 key 값이 이미 존재한다면, 해당 key에 대응되는 value 값을 업데이트

* getOrDefault(key, defaultValue) 메서드: 지정된 key 값에 해당하는 value 값을 반환합니다. 만약 key 값이 존재하지 않으면 defaultValue 값을 반환


그렇다면 이걸 문제에 적용시켜서 해결한다면 아래와 같을 것이다.



import java.util.HashMap;

import java.util.Iterator;

import java.util.Map;

class Solution_Hash {

    public String solution(String[] participant, String[] completion) {
    
        String answer = "";
        HashMap<String, Integer> map = new HashMap<>();
        for (String player : participant) 
            map.put(player, map.getOrDefault(player, 0) + 1);
        for (String player : completion) 
            map.put(player, map.get(player) - 1);

            Iterator<Map.Entry<String, Integer>> iter = map.entrySet().iterator();

        while(iter.hasNext()){
            Map.Entry<String, Integer> entry = iter.next();
            if (entry.getValue() != 0){
                answer = entry.getKey();
                break;
            }
        }
        return answer;
    }
}

HashMap을 사용해 key:value로 저장을 하는데 처음 반복할때 value에 1을 저장해주고
다음 반복할 때 completion에 있는 사람이 있다면 -1을 해주면

결과로 마지막에 participant 에 남아있는 사람의 value는 1이 될 것

## 어떤데 적용하면 좋을까?

정렬을 할 수없는 String 배열에 사용하면 굉장히 좋을 것

또한 데이터 양이 크면클수록 효율이 좋다.


```python

```
