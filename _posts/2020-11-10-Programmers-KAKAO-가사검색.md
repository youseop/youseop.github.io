---
layout: post
title: BAEKJOON#14425 문자열 집합
subtitle: Python
cover-img: /assets/img/posting/mountain.jpg
thumbnail-img: /assets/img/posting/Programmers.png
share-img: /assets/img/posting/mountain.jpg
tags: [trie, BAEKJOON]
comments: true
---

## [가사 검색](https://programmers.co.kr/learn/courses/30/lessons/60060)

Trie 자료구조를 응용한 문제이다. 아직 Trie 자료구조에 익숙하지 않다면, ['문자열 집합'](https://youseop.github.io//2020-11-09-BAEKJOON-14425_%EB%AC%B8%EC%9E%90%EC%97%B4%EC%A7%91%ED%95%A9/)이라는 문제를 통해 익숙해진 후 이 문제에 도전해보도록 하자!

### 문제분석

```
["fro??", "????o", "fr???", "fro???", "pro?"]
```

위와 같은 입력이 주어졌을 때, 각각의 문자열에 대해 매치되는 가사가 몇 개 있는지 찾아야 한다.
여기서 중요한 정보는 문자열의 길이와 "?"가 아닌 단어들이다.

"fro??"와 매치될 수 있는 단어들에 대해 살펴보자.
가사들 중 문자열의 길이가 5이고, "fro"로 시작하는 가사들이 매치될 수 있을 것이다.

따라서, Trie를 구성하는 각각의 Node에 삽입되었던 문자열의 길이 정보를 저장해 놓아야 한다.

Trie Class의 insert function에 대해 알아보자.

- 각각의 Node의 data값에 해당 노드를 지나쳐 가는 문자열의 길이를 추가해준다.
- 배열의 형태로 추가할 수도 있지만, 가사의 수가 많아지면 탐색하는데 시간복잡도가 커지기 때문에, dictionary를 사용했다.
- data에 같은 길이가 이미 저장되어있다면, 길이에 해당하는 값만 +1을 한다. 저장되어있지 않다면, 해당 길이의 값을 1로 저장해준다.

```python
def insert(self, string):
    curr_node = self.head
    length=len(string)

    # "?????"와 같은 입력을 대비해,
    # head의 data에도 문자열의 길이를 추가해준다.
    if length in curr_node.data:
        curr_node.data[length]+=1
    else:
        curr_node.data[length]=1

    for char in string:
        if char not in curr_node.children:
            curr_node.children[char]=Node(char)
        curr_node = curr_node.children[char]
        #문자열을 구성하는 문자 각각의 노드(data)에 길이 정보를 추가한다.
        if length in curr_node.data:
            curr_node.data[length]+=1
        else:
            curr_node.data[length]=1
```

search function에 대해 알아보자.

- "fro??"같은 문자열은 "?"를 strip으로 없앤 후에 검색하도록 할 예정이다. 따라서, 기존 문자열의 길이와 "?"를 제외한 문자열 두개의 값을 입력으로 받도록 했다. ex)("fro",5)
- 문자열을 구성하는 각각의 문자에 대해 Node를 타고 내려가다가, 해당 문자에 대한 Node가 없으면 0을 return
- 검색이 끝났다면, 해당 Node의 data값에서 문자열의 길이가 같은 가사의 개수 정보를 return

```python
def search(self, string,length):
    curr_node = self.head
    for char in string:
        if char in curr_node.children:
            curr_node = curr_node.children[char]
        else:
            #문자에 해당하는 node가 없으면 이 단어로 구성된 가사가 없다는 뜻이다.
            return 0
    #data에서 문자열의 길이가 같은 가사들의 개수를 찾는다.
    if length in curr_node.data:
        return curr_node.data[length]
    else:
        return 0
```

<br>

### Example

words = ["frod","kaka","fry"]
queries = ["fr?","????"]
위의 입력을 예시로 들어 그림으로 나타내 봤다.
(그림이 너무 복잡해져서 주어진 입출력을 그대로 사용할 수 없었다.)

- 먼저 "frod"를 insert하자.

![Crepe](https://i.imgur.com/VNI6h9r.jpg)

- insert("fry")

![Crepe](https://i.imgur.com/RDQWTZO.jpg)

- insert("kaka")

![Crepe](https://i.imgur.com/KlTAGwu.jpg)

- "fr?"와 매치되는 가사를 찾아보자.

- 이 단어의 길이는 3이며, ?를 제외하면 "fr"이다.
- Head의 child에 f가 존재함으로, f Node로 이동.
- f Node의 child에 r이 존재함으로, r Node로 이동.
- 문자열 탐색을 마쳤고, 현재 Node의 Data[3]=1 임으로, 매치되는 가사가 1개임을 알 수 있다.

- "????"와 매치되는 가사를 찾아보자.

- 이 단어의 길이는 4이며, ?를 제외하면 ""이다.
- 탐색할 단어가 존재하지 않음으로, Head Node에서 Data정보를 살펴본다. Data[4]=2 임으로, 매치되는 가사가 2개 있음을 확인할 수 있다.

### Plus

위의 예시에서 "?"가 오른쪽에 있는 경우만 다루었는데, "?"가 왼쪽에 있는 경우에는 어떻게 해야할까?

이를 해결하기 위해서 Trie를 두개 생성했다. 하나의 Trie는 위의 예시처럼, "?"가 오른쪽에 있는 문자들을 처리하고, 다른 하나의 Trie는 "?"가 왼쪽에 위치한 문자들을 처리한다.
두 번째 Trie에는 처음부터 가사들을 뒤집어서 insert하고, "???ao"와 같은 문자들은 "?"를 없애고, 마찬가지로 뒤집어서 search하면 해결할 수 있다.

### CODE

```python
def solution(words, queries):
    # Trie를 위한 Node 생성
    class Node(object):
        def __init__(self,key):
            self.key = key
            self.data = {} #해당 노드를 거쳐간 문자열의 길이 저장
            self.children = {}

    class Trie(object):
        def __init__(self):
            self.head = Node(None)

        def insert(self, string):
            curr_node = self.head
            length=len(string)

            # "?????"와 같은 입력을 대비해,
            # head의 data에도 문자열의 길이를 추가해준다.
            if length in curr_node.data:
                curr_node.data[length]+=1
            else:
                curr_node.data[length]=1

            for char in string:
                if char not in curr_node.children:
                    curr_node.children[char]=Node(char)
                curr_node = curr_node.children[char]
                #문자열을 구성하는 문자 각각의 노드(data)에 길이 정보를 추가한다.
                if length in curr_node.data:
                    curr_node.data[length]+=1
                else:
                    curr_node.data[length]=1

        def search(self, string,length):
            curr_node = self.head
            for char in string:
                if char in curr_node.children:
                    curr_node = curr_node.children[char]
                else:
                    #문자에 해당하는 node가 없으면 이 단어로 구성된 가사가 없다는 뜻이다.
                    return 0
            #data에서 문자열의 길이가 같은 가사들의 개수를 찾는다.
            if length in curr_node.data:
                return curr_node.data[length]
            else:
                return 0

    answer = []
    #우리가 만든 Trie자료구조를 해당 문자로 '시작'하는 단어의 개수가 몇 개인지 찾을 수 있도록 설계했다.
    #따라서, "?"가 왼쪽부터 시작하는지, 오른쪽부터 시작하는지에 따라 검색방법을 달리 해야한다.
    words_left=Trie()
    words_right=Trie()
    for w in words:
        words_right.insert(w)
        words_left.insert(w[::-1])

    for q in queries:
        #strip하기 전 미리 문자열의 길이를 저장해 놓아야 한다.
        length=len(q)
        left=True
        if q[-1]=="?":
            left=False
        q=q.strip("?")

        if left:
            #물음표가 왼쪽부터 시작하면, 문자열을 뒤집은 후 words_left에서 search한다.
            q=q[::-1]
            answer.append(words_left.search(q,length))
        else:
            answer.append(words_right.search(q,length))
    return answer
```

<br>

틀린 부분이 있을 수 있습니다. 피드백 주시면 고치도록 하겠습니다!
감사합니다.👍

[꼭 다시 풀어보기]
<br>
<br>
