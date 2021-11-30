---
title: Programmers 기능개발 (Python)
categories: Programmers
tags: [programmers, python]
toc: True
toc_label: "[프로그래머스] 기능개발"
toc_sticky: True
---

## 문제
<span style="font-size:0.9em">:computer_mouse:
<a href='https://programmers.co.kr/learn/courses/30/lessons/42586' target='_blank' style="color: #2F4F4F; font-size:0.9em">
  문제 링크(클릭)
</a>
</span><br><br>
프로그래머스 팀에서는 기능 개선 작업을 수행 중입니다. 각 기능은 진도가 100%일 때 서비스에 반영할 수 있습니다.

또, 각 기능의 개발속도는 모두 다르기 때문에 뒤에 있는 기능이 앞에 있는 기능보다 먼저 개발될 수 있고, 이때 뒤에 있는 기능은 앞에 있는 기능이 배포될 때 함께 배포됩니다.

먼저 배포되어야 하는 순서대로 작업의 진도가 적힌 정수 배열 progresses와 각 작업의 개발 속도가 적힌 정수 배열 speeds가 주어질 때 각 배포마다 몇 개의 기능이 배포되는지를 return 하도록 solution 함수를 완성하세요.

**제한사항**
<ul style="font-size: 0.8em;">
<li>작업의 개수(progresses, speeds배열의 길이)는 100개 이하입니다.</li>
<li>작업 진도는 100 미만의 자연수입니다.</li>
<li>작업 속도는 100 이하의 자연수입니다.</li>
<li>배포는 하루에 한 번만 할 수 있으며, 하루의 끝에 이루어진다고 가정합니다. 예를 들어 진도율이 95%인 작업의 개발 속도가 하루에 4%라면 배포는 2일 뒤에 이루어집니다.</li>
</ul>

### 입력 및 출력

* 예시1
```
progresses: [93, 30, 55]
speeds: [1, 30, 5]
```

* 예시1 출력
```
[2, 1]
```

* 예시2
```
progresses: [95, 90, 99, 99, 80, 99]
speeds: [1, 1, 1, 1, 1, 1]	
```

* 예시2 출력
```
[1, 3, 2]
```

## My Solution
프로그래머스 문제 재밌어..!! 뭔가 이야기가 써있으니까 백준과는 다르게 재밌다. 그리고 제출 성공하면 다른 사람이 푼 코드볼 수 있어서 좀 더 효율적으로 짤 수 있는 방안에 대해 알 수 있어서 좋은 것 같다!<br><br>
이번 문제는 기본적인 자료구조 문제였다. `deque`라이브러리를 사용하여 구현하였는데 `deque`는 progress, speed, 그리고 내가 추가해준 우선순위 정보를 담고 있다. 처음에는 우선순위 정보에 대한 내용없이 progres와 speed만 담았었는데 그렇게 되면 문제 조건에서 나온 '뒤에 있는 기능은 앞에 있는 기능이 배포될 때 함께 배포된다'라는 것을 구현하지 못했기 때문에 우선순위에 대한 정보를 담아야겠다고 생각했다. <br>
그리고 우선순위가 높은 작업이 처리되었는지 확인하기 위해 `tmp`라는 변수를 활용했는데 처음에는 0으로 초기화해두었다. 그 후 하나씩 `pop`하면서 progress를 확인해주는데 이 때 progress가 100이상이라면 `tmp`와 현재 작업의 우선순위인 `priority`를 비교하여 `priority - 1 = tmp`라면 바로 앞선 작업이 모두 배포된 것을 알 수 있기 때문에 `cnt`에 1을 더해준 후 `tmp = priority`로 `tmp`를 업데이트해준다. 만약 `priority - 1 != tmp`라면 지금 완성된 작업보다 우선순위가 높은 작업이 아직 배포되지 않은 것이기 때문에 값 그대로 다시 queue에 넣어준다. 그리고 만약 progress가 완성되지 않았다면 progress에 speed를 더한 후 다시 queue에 넣어주면 된다. <br>
마지막으로 `cnt`가 0이 아니라면 그 날 배포된 작업이 있다는 것이기 때문에 해당 날짜에 배포할 수 있는 작업 개수인 `cnt`를 answer에 추가해주면 된다.

### 코드
{% highlight python linenos %}
from collections import deque
def solution(progresses, speeds):
    answer = []
    q=deque()
    tmp=0
    for i in range(len(speeds)):
        q.append([progresses[i], speeds[i], i+1])

    while q:
        cnt=0
        q_length=len(q)
        for i in range(q_length):
            progress, speed, priority=q.popleft()
            if progress>=100:
                if priority-1==tmp:
                    tmp=priority
                    cnt+=1
                else:
                    q.append([progress, speed, priority])
            else:
                q.append([progress+speed, speed, priority])
                
        if cnt!=0:
            answer.append(cnt)
    
    return answer
{% endhighlight %}
<br>
나는 26줄로 풀었는데 다른 사람코드보니까 7줄로 푸신 분도 계셨다.. 물론 짧다고 다 좋은건 아니지만 코드도 굉장히 효율적이어서 무릎을 탁 쳤다. 여전히 배워야 할 것들이 정말 많다.(요즘 맨날맨날 느끼고 있는 거😢) 열심히 하자 효쥬야👊👊