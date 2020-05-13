---
title: (Python) 14. 파일 입출력
date: 2018-09-20T21:52:22+09:00
author: nobbaggu
layout: post
categories: Python
image: /images/2018/09/파이썬-썸네일.jpg
tags:
  - close
  - file
  - input()
  - open
  - output
  - python
  - read
  - readline
  - readlines
  - reads
  - wrtie
  - 쓰기
  - 열기 모드
  - 읽기
  - 입력
  - 입출력
  - 출력
  - 파이썬
  - 파일
---
지금까지는 출력을 입출력의 가장 기본인 키보드와 모니터 대상으로 한 입출력만 다루어왔다. 하지만 파일을 대상으로 하는 입출력 또한 많이 쓰이는 만큼 중요하기 때문에 알아야 한다. 파일이라 함은 대표적으로 텍스트파일, 이미지 파일, 음성, 영상 파일등이 있다. 프로그램이 파일을 대상으로 입출력을 한다 함은 파일에 내용을 쓰고(프로그램의 출력) 그것을 읽어들이는 것(프로그램으로 들어오는 입력)이다.

&nbsp;

# 1. 파일 생성

* * *

먼저 파일을 만들어야한다.

~~~ python
f = open("new file.txt", 'w') # 지금 작성중인 .py파일과 같은 디렉토리에 'new file.txt' 파일 생성
f.close() # 파일 닫기
~~~

open() 함수는 파일을 생성해주는 파이썬 내장함수이다. open은 입력인수로 1. 파일이름, 2. 열기 모드 를 받는다. 그리고 리턴값으로 파일 객체를 돌려준다. 파일 객체라 함은 파일에 접근하기 위한 수단 쯤으로 생각하면 된다. 우리가 a=1 처럼 변수 a에 1을 저장하고 그 값에 접근하기 위해 a라는 변수를 이용하는 것과 같다. 이제 이 객체가 담긴 f를 사용해서 파일에 접근할 수 있다. 위에서는 파일 이름 앞에 아무런 디렉토리를 입력해주지 않았기 때문에 default로 실행 파일이 있는 디렉토리에 new file이 생성된다. 디렉토리를 직접 설정해주고 싶다면 "C:/직박구리/할미새/new file.txt" 처럼 입력해주면 된다.

f.close() 는 'f가 참조하는 파일 객체에 접근해서 닫아라' 정도라 해석할 수 있다.

위에서 말한 파일 열기 모드는 읽기/쓰기/내용추가 정도의 모드가 있다.

<table style="border-collapse: collapse; width: 67.2727%;" border="1">
  <tr>
    <td style="width: 17.3939%; text-align: center;">
      <strong>파일 열기 모드</strong>
    </td>
    
    <td style="width: 49.8788%; text-align: center;">
      <strong>설명</strong>
    </td>
  </tr>
  
  <tr>
    <td style="width: 17.3939%; text-align: center;">
      r
    </td>
    
    <td style="width: 49.8788%;">
      읽기 모드 - 파일의 내용을 읽을 때
    </td>
  </tr>
  
  <tr>
    <td style="width: 17.3939%; text-align: center;">
      w
    </td>
    
    <td style="width: 49.8788%;">
      쓰기 모드 - 파일에 내용을 쓸 때
    </td>
  </tr>
  
  <tr>
    <td style="width: 17.3939%; text-align: center;">
      a
    </td>
    
    <td style="width: 49.8788%;">
      추가 모드 - 파일이 마지막에 새로운 내용을 추가할 때
    </td>
  </tr>
</table>

&nbsp;

&nbsp;

&nbsp;

# 2. 파일 쓰기

* * *

알아두어야 할 것이 있는데, 우리가 open() 함수로 파일을 생성할 때, 해당 이름으로 이미 파일이 있으면 그 파일이 삭제되고 우리가 새로 연 파일이 생성된다. 아무튼 파일을 열어 내용을 적는 방법을 보자.

&nbsp;

~~~ python
f = open("new file.txt", 'w')
f.write("sentence 1\n") # 줄 바꾸기
f.write("sentence 2") # 줄 바꾸기를 하지 않아 "sentence 3" 이어서 출력
f.write("sentence 3")
f.close()
~~~

f.wirte("sentence 1") 은 'f가 참조하는 파일에 접근해 write(쓰기)를 해라' 정도로 해석된다.

위의 코드에서 write()함수 대신 print() 함수를 써보자.

&nbsp;

~~~ python
print("sentence 1\n") # 줄 바꾸기
print("sentence 2") # 줄 바꾸기를 하지 않아 "sentence 3" 이어서 출력
print("sentence 3")
~~~

두 코드의 다른점은 프로그램의 출력 방법이다. 파일 출력에서는 프로그램의 출력 문장들이 파일에 쓰여지고 두 번째 코드에서는 프로그램의 출력 문장이 모니터를 통해 나오는 것 뿐이다. 아무튼 파일 출력 코드를 실행하면 다음 그림과 같이 파일이 생성되고 내용을 확인할 수 있다.

![image](/images/2018/09/no-name-13.png){: width="50%" height="50%"}

&nbsp;

&nbsp;

&nbsp;

# 3. 파일 읽기

* * *

외부의 파일에 접근해 내용을 읽는 여러 방법들이 있다. 아까 만들어 놓은 new file을 읽어보자.

&nbsp;

<span style="font-size: 14pt;"><strong>1) readline() 함수</strong></span>

~~~ python
f = open("new file.txt",'r') # 파일에 읽기모드로 접근해 열고 변수 f에 파일 객체 저장
sentence = f.readline() # f가 참조하는 파일의 내용을 한 줄 읽어 sentence 변수에 저장
print(sentence)
f.close()
~~~

먼저 파일을 열고 객체를 변수 f에 저장한다. 이후 파일을 읽기 위해 f.readline()함수를 사용한다. 이 명령은 'f가 참조하는 파일에 접근을 해서 내용을 한 줄 읽어들여라' 이다. 이후 내용을 sentence라는 변수에 저장한다. 그리고 그 문장을 출력하면 new file의 가장 첫 번째 라인이 출력되는 것을 볼 수 있다. 또한 f.close()를 통해 파일 f를 닫는 것을 잊으면 안된다.

파일의 모든 내용을 읽으려면 다음과 같이 코드를 작성하면 된다.

~~~ python
f = open("new file.txt",'r') # 파일에 접근해 열고 변수 f에 파일 객체 저장
while True:
    sentence = f.readline()
    if not sentence: # 한 라인을 읽었는데 공백이면 'False'를 반환해 while문 탈출
        break
    else:
        print(sentence, end='') # 파일에는 없는 빈 줄을 없애기 위해 end='' 사용
~~~

위와 같이 루프로 작성해서 파일에 더 이상 읽을 내용이 readline() 함수가 'None'을 반환하여 break을 수행하여 while문을 빠져나올 때 까지 읽어들이면 처음부터 끝 까지 읽을 수 있다. 참고로 print 함수 뒤에 end="를 넣은 이유는 print함수가 자동적으로 enter를 치기 때문에 파일의 라인들 사이에 공백 라인을 만드는 것을 없애기 위해서이다.

위의 코드에서 f.readline()을 input()으로 바꾸어주면 사용자가 입력을 멈출 때 까지 사용자 입력을 읽어들여 출력한다. 두 경우는 단지 파일의 내용이 입력으로 들어오느냐, 사용자에 의해 입력이 들어오느냐 차이만 있을 뿐이다.

&nbsp;

<span style="font-size: 14pt;"><strong>2) readlines() 함수</strong></span>

~~~ python
f = open("new file.txt", 'r')
sentences = f.readlines()
for i in sentences:
    print(i,end='')
f.close()
~~~

readlines() 함수가 readline() 함수와 다른점은 파일의 모든 내용을 읽어 들인다는 것이다. 파일의 모든 라인을 읽어들이고 각 라인을 하나의 요소로 하는 리스트를 만들어 반환시켜준다. 따라서 sentences 변수에는 다음의 내용이 저장된다. ['sentence 1\n', 'sentence 2sentence 3']

for 함수를 사용해 각각의 요소를 출력하면 된다.

&nbsp;

<span style="font-size: 14pt;"><strong>3) read() 함수</strong></span>

~~~ python
f = open("new file.txt",'r')
data = f.read()
print(data)
~~~

read() 함수는 파일의 내용 전체를 문자열로 return 시켜준다. 단순히 data를 출력해보면 파일의 전체 내용이 출력되는 것을 확인할 수 있다.

&nbsp;

&nbsp;

# 4. 파일 내용추가

* * *

처음에 파일을 'w' 쓰기모드로 열었을 때, 이미 같은 이름의 파일이 존재하면 파일의 내용을 지우고 새로 쓴다고 했다. 기존의 파일에 내용을 추가하려면 'w' 모드가 아닌 'a' 의 추가 모드로 열어야 한다.

~~~ python
f = open("new file.txt", 'a')
f.write("sentence 3\n")
f.write("sentence 4\n")
f.write("sentence 5\n")
f.close()
~~~

파일을 'a' 쓰기모드로 열고 위와 같이 f.write() 함수를 사용하면 위의 내용이 기존 파일에 있던 내용 뒤에 추가된다.

파일을 읽어 확인해 볼 수 있다.

~~~ python
f = open("new file.txt",'r')
data = f.read()
print(data)
f.close()
~~~

sentence 1


sentence 2sentence 3sentence 3


sentence 4


sentence 5

위의 결과가 나온다.

프로그램이 종료될 때는 열려있는 모든 파일을 자동으로 닫아준다. 하지만 프로그램의 종료 전에 또 다시 파일을 열려고 하면 오류가 나기때문에 f.close() 함수를 사용하는 것이 좋다. 그런데 이를 매번 쓰기 번거롭다면 하나의 방법이 있다.

~~~ python
with open("new file.txt",'w') as f:
    f.write("Python is funny!")
~~~

위 처럼 with 를 사용하면 with문을 빠져나가는 순간 열려 있던 파일 f를 닫아준다.