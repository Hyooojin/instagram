# 배포



## AWS Lightsail

------------

### 1. 인스턴스 설치

* os전용
* ubuntu

### 2. 터미널 사용하기

* 쉘 스크립트 만들어보기
* https://github.com/classjohn/server_setup 활용하여 설치

### [설치]

- Lightsail
- go Rails [https://gorails.com/deploy/ubuntu/16.04](https://gorails.com/deploy/ubuntu/16.04])
- setup 참고 [https://github.com/classjohn/server_setup](https://github.com/classjohn/server_setup)
- https://github.com/classjohn/setup_guides

Who is 리누스 토발즈?

> 리눅스의 아버지
> git 창시자로 3일만에 만든 tool이다. svn이라는 version관리 시스템이 있었지만, 불편해서 만든것이 git이다.
>

```
https://github.com/classjohn/server_setup.git
```

<br>

### 쉘 스크립트 만들어 보기

---------------------------------------------
파일 하나에 여러가지 명령어를 넣고 실행할 수 있기 때문에 설치때의 효율을 높여준다.
### 1. 스크립트 만들기
```
vi hello.sh
```

일반 유저에게 읽고 쓰기 권한을 준다.

### 2. 권한모드 바꾸기
```
sudo chmod u+x hello.sh
ls -al
sodu chmod a+x hello.sh
# -rwxrwxr-x 1 ubuntu ubuntu   25 Nov 30 01:14 hello.sh
```
1. x는 실행권한
2. 권한이 바뀐 것을 볼 수 있다.
3. 모든 유저에게 실행권한을 주겠다.
  [리눅스 권한 관리](http://cailisin.tistory.com/entry/%EC%9C%A0%EB%8B%89%EC%8A%A4-Unix-%EB%A6%AC%EB%88%85%EC%8A%A4-Linux-%EA%B6%8C%ED%95%9C-%EA%B4%80%EB%A6%AC)

### 3. 실행
```
. hello.sh
```

<br>

### 설치파일 스크립트 실행

------------

### 1. git sep_up 파일 참고해서 실행
```
$ git clone https://github.com/classjohn/server_setup.git

$ sudo chmod u+x ~/server_setup/*.sh

$ ~/server_setup/setup.sh

$ exec $SHELL

$ rbenv install 2.3.5
$ rbenv global 2.3.5
$ gem install bundler
$ rbenv rehash

$ gem install rails -v 4.2.9
```



### Nginx: 웹서버

---------



Nginx에 대해서 ..
<br>

**Web server software**
> * WEBrick
>   : Test용으로 하나의 서버에서 돌리는 경량형 server
* nginx
  : [https://opentutorials.org/module/384/3462](https://opentutorials.org/module/384/3462) <br>
  Nginx(엔진 x라 읽는다)는 웹 서버 소프트웨어로, 가벼움과 높은 성능을 목표로 한다. <br>
  최근은 nginx를 많이 사용하고 있다.
* appach

**[서버 관련된 기술]**

* phusion passenger
  [https://www.phusionpassenger.com/](https://www.phusionpassenger.com/)
  : 매우 유용한 server program이다.
* doker
  : 리눅스 가상화 서비스 (aws에서도 ecs에서 doker 지원 서비스도 있다.)
* chef
  : [https://www.chef.io/chef/](https://www.chef.io/chef/) <br>
  DevOps를 위해서 chef를 사용할 것이다.



### 1. nginx server setup

```
$ ~/server_setup/nginx.sh
$ sudo service nginx start
# 인스턴스 ID를 url 창에 붙여넣기 하면 실행이 된다.
```



### 2. 인터넷에 대해서 알고가기

참고자료

<hr>

* [How internet works](https://www.youtube.com/watch?v=Dxcc6ycZ73M&list=PLzdnOPI1iJNfMRZm5DDxco3UdsFegvuB7)

Code.org 컨텐츠를 보면 많이 도움이 된다.

* [cs 50 internet]

```
 sudo vi /etc/nginx/nginx.conf

 # 주석해제
 include /etc/nginx/passenger.conf
 # nginx의 설정파일,
```



```
 vi /etc/nginx/passenger.conf

 # https://gorails.com/deploy/ubuntu/16.04#nginx-passenger

 # 붙여넣기
 passenger_ruby /home/deploy/.rbenv/shims/ruby; # If you use rbenv


 sudo service nginx restart
```

페신저 서버에 우리 rails서버가 어디있는지 아직 알려주지 않았다.



### 3. passenger

```
sudo vi /etc/nginx/sites-enabled/default

# 날리기 -> v + shif g + x
# u

# 추가하기


```



* production으로 바꿔주고

  * secre

* gitignore
  서버에는 적용되고, local에는 적용이 안된다. <br>
  secrete.yml database.yml

* rake assets:precompile
  sudo service nginx restart

  reload가 필요하다.
  만약 안되면,
  touch tmp/restart.txt(passenger reload가 필요한경우)
  ​


* 서버에서 migrate 하는 방법
 - rake db:migrate RAILS_ENV=production
 - rake assets:precompile 실행
 - touch tmp/restart.txt
 - sudo service nginx restart 로 서버 재실행

sca





### 4. capistrano


우리가 이런 Program을 쓰지 않으면 **노가다** 이다.
이런 노가다를 하지 않기 위해서 <br>
[1단계]: 쉘 스크립트를 이용하는 것이다.  <br>
하지만, 이것도 문제가 있다.
[2단계]:  chef cookbook을 사용하면 간단하게 쓸 수 있다.
