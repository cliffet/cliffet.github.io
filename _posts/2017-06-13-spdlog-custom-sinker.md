---
layout: post
title: spdlog 라이브러리를 사용할 때 커스텀 싱커 구현
category: dev
tags: [c++, dev]
---

## spdlog 라이브러리 로깅 시스템 구조
### logger
로그를 남기는 인터페이스를 제공하는 객체. 각 logger객체마다 sink와 async_log_helper객체를 하나씩 가진다.
### sink
로그 데이터를 정해진 대상에 쓰기를 담당. 기본적으로 콘솔,파일에 쓰는 sink는 라이브러리에서 기본 제공하며
기타 DB나 네트웍으로 쓰는 sink는 상속을 통해 커스텀하게 구현할 수 있다.
### async_log_helper
워커 쓰레드, 큐, sink들을 묶어준다. logger가 로그를 쓰면 이 로그 데이터는 큐(lockfree 알고리즘으로 구현)에 추가하는
것으로 끝난다. 워커 쓰레드는 이 큐에 추가된 데이터를 하나씩 꺼내 등록된 모든 sink들로 하여금 이 로그데이터를 처리할 수 있도록 한다.
참고로 logger를 처음 생성할 때에는 기본적으로 하나의 sink를 생성하지만 logger가 생성된 이후 추가로 sink를 등록할 수 있다.

## thread-safety
위 구조에 따르면 서로 다른 logger들은 같은 sink 인스턴스를 공유할 수도 있다. 그런데 각 logger마다 워커가 있고 이 워커쓰레드에서
공유중인 sink를 동시에 접근할 수 있으므로 동기화 메커니즘이 필요함을 알 수 있다.  
요약하면 sink를 공유한다면 쓰기 작업은 락을 걸어서 안전하게 처리될 수 있도록 해야 한다.

### spdlog::sinks::sink vs spdlog::sinks::base_sink<LockType>
spdlog::sinks::sink는 싱글쓰레드에서 사용되는 sink들의 기본클래스이고 멀티쓰레드에서는
spdlog::sinks::base_sink<LockType>을 상속받아 sink를 구현해서 동기화를 보장하도록 해야 한다.
