<!-- ---
layout: post
title: Process
author: "SangKyeong Lee"
tags: Operating-System
---

# Process
process란 프로그램을 실행한 모든 경우를 지칭한다.
- java에서 class는 program이며, 객체를 생성한 경우 process라고 할 수 있다.

각각의 process들은 자신만의 ID를 가지며 그것을 `PID`라고 부르며, PID를 통해 식별된다.

## Process in Memory
모든 process들은 메모리에서 자신만의 고유한 주소공간을 가진다.
- Code 섹션은 프로그램의 코드를 포함한다.
    - Program Counter가 현재 실행중인 instruction을 가리키고 있다.
- Data 섹션은 전역 변수를 포함한다.
- Stack 섹션은 지역 변수, 함수 파라미터, 리턴값 주소등의 temporary한 데이터를 포함한다.
- Heap 섹션은 실행 중에 동적으로 할당된 메모리를 포함한다.

## Process State Transitions
Process가 실행되면 상태가 바뀌는데 다음과 같다.
- new: process가 생성되고 있다.
- running: instruction이 실행되고 있다.
- waiting: process가 다음 발생할 event를 기다리고 있다.
- ready: process가 processor(CPU)에게 할당될 준비가 됐다.
- terminated: process가 실행을 끝마쳤다.

![08](/assets/os/08.png)

## fork()
직역: 현재 프로세스의 두개로 나누는 것<br>

fork()는 현재 프로세스를 복제하여 새로운 프로세스를 만드는 것을 말한다.
- 부모의 대부분의 리소스와 권한 등을 모두 상속한다.
- 자식은 부모의 주소 공간을 복사한다.

## exec()
<img src="/assets/os/09.png" align="left" width= 400 height = 200><img src="/assets/os/10.png" align="right" width = 400 height = 200>
<br>
위 그림 처럼 fork()를 하면 그대로 복사가 되는데 exec()를 실행하면 복제 된 process가 하던 작업이 새로운 작업으로 완전히 대체된다.
- 현재 주소공간이 완전히 날라가고 새로 재구성 된다.

## Process 종료
process종료는 `자발적 종료`와 `비자발적 종료`로 나뉜다.

자발적 종료
- normal exit
- error exit

비자발적 종료
- fatal error
    - segmentation fault
    - protection fault
    - exceed allocated resources
- 다른 process의 signal에 의한 종료

Process는 `exit()` System call을 사용해서 OS에 스스로 삭제하겠다고 요청을 하고 해당 process의 부모는 `wait()`를 통해 자식의 상태 데이터(리턴 코드)를 리턴받는다.

부모 process는 자식 process를 `abort()`나 `kill()` System call을 사용해서 종료시킬 수 있다.
- 비동기적 IPC 메커니즘인 signal에 의해서 제어된다.
- 부모 process도 `abort()`에 의해서 종료될수 있음.

### Zombie process, Orphan process
Zombie process는 리턴 코드(실행 결과)를 남긴 채 종료되었는데, 아직 부모 Process가 종료된 것을 알지 못하는 것을 말한다.
- 부모 process가 wait()를 실행하여 실행 결과를 회수하며, 그 후에 Zombie process의 정보는 사라진다.

Orphan process는 부모 process가 종료되어 자식 process의 리턴 코드를 회수할 수 있는 wait() 시스템 콜을 부르지 못하는 경우를 말한다.
- 해결방법으로 몇몇 OS는 해당 부모가 죽을 시, 그 자식들을 모두 종료 시킨다.
- Linux 및 Unix에서는 해당 process의 부모를 다시 정하는 방법을 사용하는데, `init/systemd process`를 부모 process로 정한다. -->
