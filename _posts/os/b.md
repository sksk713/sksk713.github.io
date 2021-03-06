<!-- ---
layout: post
title: System Call
author: "SangKyeong Lee"
tags: Operating-System
---

# Protectin the System
CPU는 `user mode`, `kernel mode`라고 하는 두가지의 모드를 가진다.
- `privileged instructions`(중요한 inst)는 오직 kernel mode에서만 작동 가능하다.
- mode를 구분하는 bit는 hardware에 의해 제공된다.

## Privileged Instructions
신중하게 사용되야 하는 instruction들의 집합이다.
- Direct I/O access (IN/OUT instructions in IA-32)
- Accessing/manipulating system registers (Interrupt descriptor table, Control registers)
- Memory state management (page table updates, TLB loads)
- HLT instruction in IA-32 (강제 종료)

> user mode에서 해당 inst를 실행하게 되면 exception이 발생한다.

# Interrupt vs Exception
Interrupt
- hardware에서 발생한다.
- 비동기적으로 발생한다.
- `Interrupt handler를 실행하기 위해서는 kernel mode여야 한다.`

Exception
- instruction을 실행하는 software에서 발생한다.
- 동기적 (CPU가 instruction을 실행할 때 발생하기 때문에)
- Trap(expected, intended) or fault(unexpected)
- interrupts처럼 다룬다.

# Sytsem Call
시스템콜은 OS가 제공하는 서비스에 대한 프로그래밍 인터페이스다.

보통 프로그램들은 직접적으로 시스템 콜을 사용하기 보다 high-level Application programming Interface(API)를 통해 접근한다.
- Win32 API for Windows variants
- POSIX API for POSIX-based systems
- JAVA API for JVM

![04](/assets/os/04.png)
<img src="/assets/os/05.png" align="left" width= 350 height = 200><img src="/assets/os/06.png" align="right" width = 350 height = 200>
<br>
<br>

# Operating System Structure
## Simple structure - MS-DOS
- Protect가 불가 (user mode, kernel mode가 없어서 애플리케이션이 Hardware에 직접 접근이 가능했다.)
- 모듈화 되어있지 않다.
- 약간의 구조로 나뉜다 해도, 인터페이스와 기능이 분리되어 있지 않다.
- `프로그램이 OS위에서 돌아가는게 아니라 함께 돌아감`

## Layered Approach
- OS가 몇개의 layer로 구성되어 있으며, 인접한 layer들 간에만 통신이 가능하다.
- layer 0이 hardware이고 가장 높은 layer가 user interface이다.
- System call마다 얼마나 깊은 layer의 서비스 및 기능을 사용할 지 다 다름
- user interface에서 hardware까지 가는데 너무 오래걸림

## UNIX
<img src="/assets/os/07.png" align="left" width= 800 height = 500>

- 단순하지만 완전히 계층화되지는 않았지만 성능이 매우 좋다.<br>
`but..`
- kernel mode에 너무 많은 코드가 복잡하게 되어있어서 업데이트 및 유지하기가 매우 쉽지 않다.
- 서로 상호의존적이며 복잡하게 엉켜있어서 하나의 디바이스 드라이버가 모든 시스템을 망가뜨릴 수 있다.
    - 모든 OS code가 하나의 메모리 공간에서 실행된다.

## MicroKernel
kernel에 핵심적인 요소만 구현하고 나머지 요소들은 user mode에 구현하며, message를 주고받으며 통신한다.

장점
- 확장이 쉽다.
- 좀더 신뢰적이고 안전하다.
    - kernel mode에 코드의 양이 줄어서 서로 덜 상호의존적임
    - 상호의존적이지 않기 때문에 테스트를 좀 더 디테일하게 할 수 있다.

단점
- user mode에 다 빼서 구현했기 때문에 소통하기 위해 메시지를 보내는 것들의 overhead가 매우매우 크다.

## Modules
동적으로 필요한 기능만 그때그때 불러온다. 이것을 `loadable kernel modeules`라고 한다.

최근 OS들은 모두 순수한 모델이 아니고 하이브리드 모델이다.
- 리눅스는 monolithic하지만 기능 및 module들의 동적 로딩을 지원한다.
- 윈도우도 monolithic하지만 subsystem personalities를 위한 microkernel을 제공한다. -->