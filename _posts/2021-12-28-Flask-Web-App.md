---
layout: post 
title: Practice Flask, Nginx, and Docker  
date: 2021-12-28
tags: [study, flask, uwsgi, docker, nginx, socket, unix]
toc: true
---

## Python Web application!

친구가 완성한 웹 사이트 하나를 안정적으로 서비스 하기 위해서 nginx 와 uwsgi 를 이용해 배포하는 것을 도와주었습니다.
이 웹은 python 으로 작성 되었으며 flask 라는 micro web framework 를 활용했습니다.

이렇게 작성된 서버를 nginx 와 uwsgi 를 이용하여 안정적인 트래픽 처리가 가능하도록 배포하고,
이러한 배포 작업이 자동화 될수 있도록 docker 를 활용했습니다.

### What is uwsgi?
uwsgi 는 wsgi 의 일종으로 wsgi 는 web service gateway interface 의 약자 입니다.
http request 를 python 으로 작성된 어플리케이션이 처리 가능한 형태로 바꿔주고 다시 python app 에서 받은 정보를 http response 로 내보는 역할을 하는 정도로 알고 있습니다.

### What is nginx?
nginx 는 가볍고 성능이 좋은 웹 서버 소프트웨어 입니다.
apache http server 와 비교가 많이 되는데, 일전에 django 배포를 할때 gunicorn 과 함께 사용해본 경험이 있어서 친구에게 추천을 해주었는데,
대량의 트래픽을 동시에 처리하는데 있어서 nginx 의 역할이 중요합니다.

https://whatisthenext.tistory.com/123 이 포스팅이 이해하는데 도움이 되었고, 추가적인 정보도 있으니 한번 확인해 보시면 좋을것 같습니다.

### Why docker?
Server 환경에 구애 받지 않고 배포가 가능한 docker 가 각광을 받고 있고, 거의 업계 표준이기 때문에 이번기회를 삼아 한번 연습해 봤습니다.

### Lesson and Learn
uwsgi 설정이나 nginx 설정을 사실 별로 할게 없습니다.
https://github.com/hyungi/dailymotion 이 repo 에서도 확인하실 수 있듯, 기존의 이미지를 활용하면 정말 쉽게 구축이 가능합니다.

다만, 문제는 uwsgi 와 nginx 간의 통신이 문제 였는데, 속도를 극대화 하기 위해서 port 를 통한 통신이 아닌  unix socket 을 활용하고자 했는데,
초반에 docker 의 volume 에 대한 이해가 부족하여 시간이 좀 걸렸습니다.

아래는 실제 파일중 일부입니다.
```yaml
services:
    flask:
    ...
        volumes:
          - ./flask/socket:/app/socket

    nginx:
        ...
        volumes:
          - ./flask/socket:/app/socket
        ...
```

volumes 에 argument 들이 각각 의미하는 바는
{host-path}:{container-path} 입니다.

이를 정확하게 이해하지 못해 container-path 만 통일하고 host-path 를 각기 다르게 지정하여 container 내부에서 바라보는 path 만 같고 결국 다른 file 을 바라보고 있어 socket 통신이 번번이 실패했습니다.

다른 socket 을 활용한 게시물을 참고해서 문제점을 파악하고 수정했습니다.
refer: https://velog.io/@yh20studio/Docker-Django-Nginx-uwsgi-로컬서버에서-배포-진행
