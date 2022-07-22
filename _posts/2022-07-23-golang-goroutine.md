---
layout: post
title: golanggoroutine
date: 2022-07-23 00:59
tags: [golang,goroutine,goroutine leak]
toc: true
---

# Goroutine 이란?
Go 언어에서 동시성 처리를 위해 사용하는 개념
Thread 와는 다르게 Go runtime 에서 관리하기 때문에
Lightweight thread 라고 불린다.

# 무슨 문제라도?
최근에 시스템에 thread 갯수가 지나치게 많다는 리포트가 있었는데,
goroutine 은 위에서도 설명해두었 듯, Lightweight thread 이기 때문에
System 에서 관측되는 thread 와 1대다 매칭이 된다.
즉 현재 시스템에서 관측되는 thread 보다 훨씬 더 많은 goroutine 이 돌고 있을것이라는 추정이 가능했다.

# 이유는?
부끄럽게도 미숙한 goroutine 처리 때문이었다.
통상적으로 동시성처리를 위해서 `waitGroup` 또는 `channel` 을 이용하는데, 구현했던 코드 상에서는 `channel` 을 사용하였는데, 이 과정에서 `context` 를 생성해 goroutine 에게 전달 했고, 해당 `context` 를 회수 하는 것 만으로 생성한 goroutine 이 정리될 것이라고 잘못 생각하고 있었다.

```golang

func parent() {
    ctx, cancel := context.Context(context.Background())
    defer cancel()
    rc := make(chan bool, count)
    for i := 0; i < count; i++ {
        go child(ctx, rc)
    }
    t := time.NewTimer(time.Second * 10)
    for {
        select {
            case msg1 := <- rc:
                fmt.Println("Job is Done")
            case <-t.C:
                cancel()
                fmt.Println("Time out")
        }
    }
}

func child(ctx context.Context, rc chan bool) {
    // Do something and just send through channel
    rc <- true
}

```

대략 이러한 방식으로 코드를 구성 했고, `defer cancel()` 를 통해서 최종적으로 `context` 를 취소하고, timeout 이 발생했을 때 도 `context` 를 취소하기 때문에 때문에 괜찮을 것이라고 착각했었다.

그러나 실상은 이러한 형태의 구현은 goroutine leak 을 야기 했다.
`cancel()` 의 호출은 각 goroutine 에 `context` 가 취소 되었음을 알리는데 그친다. 이 호출을 통해 goroutine 들이 자동으로 정리 되는 것이 아니다. 즉 위 구현에서 `child()` 함수에 아주 중요한 부분이 빠진 것이다.

```golang
// Fix child function
func child(ctx context.Context, rc chan bool) {
    select {
        case <-ctx.Done:
            return
        default:
        // Do something
            rc <- true   
    }
}
```

결국 위와 같이 최소한 select 문을 이용해서 ctx.Done 을 감지하고 return 을 해줘야 goroutine 이 정리가 되는 것이다.

꽤나 오랜 시간 어플리케이션을 동작 시켰음에도 트래픽이 많지 않은 탓인지, 아니면 goroutine 자체가 경량화 된 솔루션인 탓인지 무엇인지는 몰라도 서버 성능 자체에 큰 문제가 없어서 눈치채지 못하다가 트래픽이 늘어자마자 문제가 관찰 되어 어쩌면 매우 다행이라고 볼수도 있겠다.