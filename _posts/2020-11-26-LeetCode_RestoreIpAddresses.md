---
layout: post
title: LeetCode Restore IP Addresses
subtitle: Python
cover-img: /assets/img/posting/mountain.jpg
thumbnail-img: https://i.imgur.com/7TKSpik.png
share-img: /assets/img/posting/mountain.jpg
tags: [DFS, LeetCode]
comments: true
---

## [Restore IP Addresses](https://leetcode.com/problems/restore-ip-addresses/)

<br>

숫자로 이루어진 문자열이 주어졌을 때, ip주소로 가능한 값들을 찾아내는 문제이다.
DFS를 사용해 풀었고, ip주소로 가능하지 않은 값들을 중간에 걸러주었다.

- Consists of exactly four integers
- Each integer is between 0 and 255
- Separated by single dots
- Cannot have leading zeros

위의 조건들을 모두 만족시켜주어야 한다.

재귀함수를 사용해 DFS를 구현했다.
함수의 인자로는

- ip - 생성중인 IP Address
- left - 주어진 input 중에서 아직 사용되지 않은 숫자들
- cnt - four integers 중 몇 번째 integer 까지 생성했는지 저장

먼저 재귀함수 탈출 조건에 대해 생각해 보았다.

- cnt가 4가 되어 four integers 중 네개를 모두 채운 경우
  - 주어진 input을 모두 사용한 경우 answer에 추가한다.
  - input을 모두 사용하지 않은 경우 추가하지 않는다.
- cnt가 4보다 작지만, 주어진 input을 모두 사용한 경우 answer에 추가하지 않고, 함수를 종료한다.

다음으로, 문제에서 주어진 조건들에 대해 생각해보자.

- Each integer is between 0 and 255
  - 3자리 수를 추가할 경우 255이하인 경우에만 재귀함수를 실행한다.
- Separated by single dots
  - 숫자를 추가한 경우 그 뒤에 '.'도 함께 추가한다.
  - 단, 4번째 숫자를 추가하는 경우에는 '.'를 생략한다.
- Cannot have leading zeros
  - 추가하는 수의 첫 번째 자리가 0인 경우, 한 자리 숫자('0')만 추가한다.
  - '0'으로 시작하는 두자리수, 세자리수는 추가할 수 없다.

### CODE

```python
class Solution:
    def restoreIpAddresses(self, s: str) -> List[str]:
        answer = []

        def VALID_IP(ip, left, cnt):
            if cnt == 4:
                #주어진 입력을 모두 사용해 4개의 칸을 채웠을 때만, answer에 추가해준다.
                if left == '':
                    if ip not in answer:
                        answer.append(ip)
                return
            #4번째 칸 까지 채우지 못했는데, 추가할 수 있는 숫자가 없는 경우 DFS탐색을 종료한다.
            elif cnt < 4 and left == "":
                return

            #'.'뒤로 위치하는 숫자가 0인 경우
            if left[0] == '0':
                if cnt == 3:
                    VALID_IP(ip+'0', left[1:], cnt+1)
                else:
                    VALID_IP(ip+'0.', left[1:], cnt+1)
            #'.'뒤로 위치하는 숫자가 0이 아닌 경우
            else:
                #마지막 칸인 경우 뒤에 '.'을 찍지 않는다.
                if cnt == 3:
                    VALID_IP(ip+left[:1], left[1:], cnt+1)
                    VALID_IP(ip+left[:2], left[2:], cnt+1)
                    #세자리 숫자를 추가할 경우 255이하 인지 확인해준다.
                    if int(left[:3]) <= 255:
                        VALID_IP(ip+left[:3], left[3:], cnt+1)
                #마지막 칸이 아닐 경우 숫자와 함께 '.'도 추가해준다.
                else:
                    VALID_IP(ip+left[:1]+'.', left[1:], cnt+1)
                    VALID_IP(ip+left[:2]+'.', left[2:], cnt+1)
                    #세자리 숫자를 추가할 경우 255이하 인지 확인해준다.
                    if int(left[:3]) <= 255:
                        VALID_IP(ip+left[:3]+'.', left[3:], cnt+1)

        VALID_IP('', s, 0)
        return answer
```

<br>

틀린 부분이 있을 수 있습니다. 피드백 주시면 고치도록 하겠습니다!
감사합니다.👍

[꼭 다시 풀어보기]
<br>
<br>
