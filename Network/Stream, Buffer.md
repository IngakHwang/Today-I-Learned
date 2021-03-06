# Stream, Buffer

![img](https://blog.kakaocdn.net/dn/bDoLXt/btrmf4Ak3Q9/3WkR4wSTAIIg7U2JlEKRkk/img.png)



파일은 mp3 처럼 4분 30초 짜리 음원 데이터 전체가 기록되어 있다.

따라서 고정되어 있는 크기의 데이터를 처음부터 끝까지 읽어서 처리할 수 있다.



하지만 스트림은 파일과는 다르다.

스트림이란 계속해서 흘러 들어오는 데이터를 의미한다.

즉, 마이크에 대고 하는 이야기, 비디오 카메라로 찍는 동영상 처럼 입력장치를 통해 계속해서 들어오는 데이터의 흐름을 의미한다.



따라서 실제 데이터가 입력되기 전에는 어떤 데이터가 들어올지 알 수 없으며, 실시간으로 들어오는 데이터를 그 때 그 때 읽고 처리해야 한다.

혹은 유튜브 처럼 대용량의 데이터를 서버를 통해 받아서 재생하는 동영상의 경우, 데이터를 모두 다운로드 받아서 재생하려면 긴 딜레이가 발생하게 된다.

이를 막기 위해서는 먼저 들어오는 데이터부터 조금씩 메모리의 임시공간에 저장하여 재생시켜 주어야 한다.





## Stream

> Buffer를 이용해 흘러 들어오는 데이터를 처리(입력 & 출력) 하는 객체

![img](https://blog.kakaocdn.net/dn/d7wi3D/btrmfue1kHb/Wlaw1AqGNQg97slCoFkGN1/img.png)

프로그래밍에서 Stream은 

마이크와 같은 입력장치나 서버로 부터 들어오는 일련의 데이터 흐름을 읽어 들이면서, 

스피커와 같은 출력장치나 동영상 플레이어로 데이터의 흐름을 내보내는 역할을 하는 "객체" 라고 볼 수 있다.



Stream이 있기 전까지는 데이터 전체가 다운로드 되어야만 처리가 가능했다.

Stream이 가능해지면서 동영상 스트리밍, 오디오 스트리밍 등의 서비스가 가능해진 것이다.



그렇다면 컴퓨터는 계속해서 흘러 들어오는 데이터들을 정확히 어떤 방식으로 처리 할까?

여기서 바로 버퍼(Buffer) 가 출현한다.



## Buffer

> Buffer란 특정 용도로 활용하기 위해 임시로 저장하는 Binary 형태의 데이터 저장 객체
>
> Stream을 위해 데이터를 chunk 단위로 쪼개어 임시로 저장해 두는 객체

![img](https://blog.kakaocdn.net/dn/cfgNCZ/btrmajy0dTj/R8a7xnW2FiK6pmjFwx3Wa1/img.png)

Buffer는

일반적으로는 Stream 데이터를 조금씩 읽고, 저장하고, 처리하고, 비우기를 반복하는 메모리 공간이다.

이런 행위를 버퍼링이라고 하며,

임시로 데이터를 저장하는 메모리 공간 혹은 데이터 자체를 버퍼라고 한다. (일반적으로 RAM 메모리의 일부공간이 버퍼를 위해 사용된다.)



즉, Buffer란 특정 용도로 활용하기 위해 임시로 저장하는 Binary 형태의 데이터 저장 객체이다.



## Stream, Buffer 관계



ex) "0 1 2 3 4 5 ... 99" 라는 동영상 데이터가 스트리명 된다면, 0부터 1,2,3 의 순서로 데이터들이 흘러 들어 올 것이다.



컴퓨터가 데이터를 처리하려면 일단 메모리에 올려두어야 한다.

메모리에 올라온 데이터만 CPU가 읽어서 처리 할 수 있기 때문이다. (책상에 부품들을 올려놔야 조립할 수 있듯이)



컴퓨터는 이 데이터들을 chunk (데이터를 처리하는 임의의 크기) 단위로 쪼개서 임시 공간 (메모리 : RAM의 일부 영역)에 저장해두고 처리를 시작한다.

그리고 처리가 완료되면 임시 공간을 비우고, 다음 데이터가 들어오길 기다린다.



즉, 한 번에 10개씩 (=chunk) 처리하기로 정했다면, 먼저 0~9 까지 10개를 입력받아 메모리에 저장하고, 동영상 형식으로 변환시켜 비디오 플레이어로 재생하고 메모리를 비운다.

0~9 데이터가 동영상으로 재상되고 있는 동안 그 다음 10~19 까지를 입력 받아 메모리에 저장하고 처리하고 또 비운다.



이렇게 하면 어마어마하게 큰 데이터도 조금씩 조금씩 처리할 수 있는 매커니즘이 완성된다.



Stream을 위해 데이터를 저장해두는 임시공간이 바로 Buffer이고,

Buffer가 작동하는 방식을 buffering(버퍼링) 이라고 한다.

따라서 버퍼의 크기는 chunk(데이터 처리 단위)와 같은 수준으로 설정되는 게 일반적이다.



----

참고사이트 

티스토리 : https://curryyou.tistory.com/440