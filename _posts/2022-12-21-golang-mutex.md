---
layout: post
title: About mutex in golang
date: 2022-12-21 21:01
tags: [golang,mutex,go]
toc: true
---

# Mutex 란?

상호 배제는 동시 프로그래밍에서 공유 불가능한 자원의 동시 사용을 피하기 위해 사용되는 알고리즘으로, 임계 구역으로 불리는 코드 영역에 의해 구현된다.

출처: 위키피디아

# golang 에서는?

https://pkg.go.dev/sync#Mutex

여기에 구현되어 있다.

## 만났던 문제

goroutine 이 급증하는 현상이 있어서 trace 를 찍어봤더니 *by https://pkg.go.dev/net/http/pprof*

Read Lock 을 거는 지점에서 대부분의 goroutine 이 block 되어 있었음.

## 문제의 원인

알 수 없는 이유로 ~~내가 분명 리뷰 했건만...~~ Read Lock 을 하나의 logic 안에서 두번 부르고 있었다.
또한 동시에 다른 goroutine 에서 Write Lock 을 주기적으로 걸 수도 있는 상황이었다.

Read Lock 은 서로 를 blocking 하지 않는데, 두번 걸었던 것이 왜 문제가 되느냐...

## 문제 현상의 발생 과정
아래와 같은 순서로 Mutex 에 각각 접근 하게 되면 dead lock 상황이 된다.
1. 첫번째 goroutine 이 Read Lock 을 건다
2. 두번째 goroutine 이 Write Lock 을 건다
3. 첫번째 goroutine 이 두번째 Read Lock 을 시도 한다. 하지만 아래와 같은 이유로 Lock 을 걸지 못하고 blocking 된다
```
# https://pkg.go.dev/sync#RWMutex
If a goroutine holds a RWMutex for reading and another goroutine might call Lock, 
no goroutine should expect to be able to acquire a read lock until the initial read lock is released. 
In particular, this prohibits recursive read locking.
```
4. 이후 시도 되는 모든 Read Lock 이 blcoking 된다

3번 단계에서 첫번째 goroutine 은 dead lock 에 빠진다고 보면 된다.
통상적으로 golang 에서 mutex 는 거는 시점에 defer 를 불러서 이후 작업이 완료되면 lock 이 해제될 수 있도록 구현 하는데, 첫번째 Read Lock 은 당연하게도 두번째 Read Lock 이 풀린 이후에 풀리도록 구현이 되어있기 때문에 두번째 Read Lock 은 자신보다 나중에 풀리도록 의도된 첫번째 Read Lock 이 풀리기를 기다리는 셈이 된다.

그래도 Write Lock 이 풀리면 알아서 풀려야 하지 않겠냐고?

그렇지 않다. 위의 예시는 아주 멋진 Dead Lock 이기 때문에 그렇게 쉽게 풀리지 않는다. 왜냐..

첫번째 Read Lock 은 두번째 Read Lock 의 해제 이후에 실행 되는데
두번째 Read Lock 은 첫번째 Read Lock 의 해제를 기다리고 있다.

이는 DeadLock 의 4가지 필요조건을 멋들어지게 만족한다.

1. 상호배제
2. 점유와 대기
3. 비선점
4. 환형 대기

~~근데 사실 이거 프로세스 두개가 서로 얽혀 있는건 아니긴 한데...?~~


## 문제의 해결
쓰잘데 없이 Read Lock 을 같은 로직 내에서 두번 걸지 말고 한번만 걸면 된다.
즉 위에서 설명한 4가지 필요조건 중 2, 4번을 해결한다고도 볼 수 있다.

## 오늘의 교훈
코드 리뷰를... 잘하자 :disappointed_relieved: