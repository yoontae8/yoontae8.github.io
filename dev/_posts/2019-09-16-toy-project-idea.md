---
layout: post
title: 토이 프로젝트 구상
categories: dev
tags: project
---

내 기술 스택 또는 배우고 싶은 기술을 사용해서 토이 프로젝트를 하나 진행하려고 생각중이다. 우선 대충 생각해본 아키텍쳐는 다음과 같다.

* back-end: Spring boot with kotlin
* database: MySQL
* cache: redis cluster
* message queue: activemq
* front-end: Android with kotlin

Spring boot와 redis cluster, activemq는 모두 이전 회사에서 일할 때 마지막 프로젝트에서 사용한 기술들이다. kotlin은 회사를 나오기 직전에 잠깐 공부했지만 괜찮은 언어라고 생각이 되어 서버 언어를 java 대신 kotlin으로 도전해 보려 한다. MySQL은 마지막 프로젝트에서 couchbase를 사용하느라 어색해진 느낌이 있어 복습하는 느낌으로 사용할 예정. front-end는 원래 flutter를 고민했었는데, 토이 프로젝트인데 굳이 양쪽 플랫폼을 모두 고려할 필요가 있을까라는 생각과 함께 이왕 kotlin 공부하는 거 Android에도 사용해보자라고 생각하게 되었다.

참 공부할 게 많다! 취업 전에 끝날지, 다 완성하지 못하고 취업하게 될 지 모르겠지만 추후에라도 조금씩 해서 꼭 완성시키고 싶다.

문제는 어떤 서비스를 만드느냐인데... 우선 요즘 스터디를 많이 하다 보니 스터디 모집&운영 앱을 만들어볼까 했지만 실효성에 대해 의문이 가서 조금 더 고민해 볼 예정이다. 우선은 공부를!