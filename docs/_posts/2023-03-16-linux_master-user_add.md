---
title: 리눅스 사용자 계정 추가하기 
category: linux
author: Bure5kzam
tags: [linux, user]
summary: 리눅스 사용자 계정 
---


## useradd

시스템에 계정을 생성하는 명령어 입니다.

지정된 옵션이 없으면 `/etc/default/useradd` 파일의 설정을 디폴트로 참고합니다.

`useradd` 명령어로 기본 설정도 수정할 수 있습니다. (-D, default, 하단의 default 설명 참고)


```
useradd [options] LOGIN
useradd -D
useradd -D [options]
```

> 참고 : man useradd 


### 홈 디렉토리 관련 옵션

유저 생성시 디렉토리 생성 여부를 선택할 수 있습니다. (-m 옵션 사용)  기본 동작은 `/etc/login.defs` 파일의 `CREATE_HOME` 필드 값에 따라  동작하지만 (`enable`/`no`), 커맨드 옵션 설정이 우선시됩니다.

홈 디렉토리 생성시 미리 정의된 구조를 복제해서 생성할 수 있습니다. (-mk 옵션 사용)

사전에 존재하는 디렉토리를 홈으로 지정할 수 있습니다 (-d로 경로 지정)

홈 디렉토리를 `/home/` 하위 경로 디렉토리로 싶지 않다면 경로를 변경할 수 있습니다. (-b로 지정)



| 주요 옵션                 | 내용                                                                                                                                                                                                                                                                                                          |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|                           |                                                                                                                                                                                                                                                                                                               |
| -m, --create-home         | home_dir 를 새로 생성하는 옵션. <br/> `-k` 옵션과 함께 사용해 디렉토리 구성을 참조할 skeletone_dir 경로를 명시해야함.  <br/> `-m` 옵션이 없고 CREATE_HOME이 no 이면 홈디렉토리가 생성되지 않습니다.                                                                                                           |
| -k, --skel  _SKEL-DIR_    | `-m`와 함께만 사용되는 옵션. skeletone_dir 경로 지정. <br/> 기본값은 `/etc/default/useradd`의 `SKEL` 필드 값이나 `/etc/skel`                                                                                                                                                                                  |
| -d, --home-dir _HOME-DIR_ | 원래 존재하는 디렉토리를 홈으로 지정하는 옵션. HOME_DIR 경로에 디렉토리가 없으면 에러가 발생하기 때문에 폴더를 생성하는 `-m` 옵션을 사용해야 합니다.<br/> 기본값은 BASE_DIR에 LOGIN name을 합쳐 login directory name으로 사용합니다.                                                                          |
| -b, --base-dir _BASE-DIR_ | 홈 디렉토리를 생성할 상위 디렉토리를 지정하는 <br> 옵션. 홈 디렉토리를 새로 생성하지 않는 `-d` 와는 함께 사용되지 않습니다.                                                                                         ~~`-m`도 없다면 꼭 필요합니다. <br/> 기본값은 `/etc/default/useradd`의 HOME 필드입니다.~~ |


### UID 관련 옵션

유저 생성시 부여할 UID를 직접 지정할 수 있습니다. (-u)

중복되게 생성할 수도 있습니다. (-o)

| 주요 옵션        | 내용                                                      |
| ---------------- | --------------------------------------------------------- |
| -u, --uid        | uid 지정 <br/> `-o` 옵션과 함께 쓰면 unique하지 않아도됨. |
| -o, --non-unique | UID를 중복 허용                                           |


### GID 관련 옵션

유저의 `primary group`을 지정할 수 있습니다. (-g로 지정)

또는 유저와 같은 이름의 group을 생성해서 `primary group`로 지정할 수 있습니다. (-U 옵션 추가)

그룹을 디폴트 값으로 지정할 수 있습니다. (-N)

계정의 보조 그룹들을 지정할 수 있습니다. (-G)


| 주요 옵션                         | 내용                                                                                                                                                                                                                                                                                                                                       |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| -g, --gid _GROUP_                 | 로그인시 적용될 그룹을 지정합니다. 기본값은 `/etc/login.defs`의 `USERGROUPS_ENAB`와 커맨드 옵션 `-U` 여부에 따라 다르게 동작합니다. <br/> yes이거나 `-U, --user-group`와 함께 사용 - 유저 이름과 같은 그룹이 생성되고 적용됩니다. <br/> no 거나 `-N, --no-user-group`과 함께 사용 - `/etc/default/useradd`의 `GROUP` 필드 값을 적용합니다. |
| -U, --user-group                  | 그룹값을 유저 이름과 동일하게 설정합니다.                                                                                                                                                                                                                                                                                                  |
| -N, --no-user-group               | 그룹값을 `/etc/default/useradd`의 `GROUP` 필드를 참조해 지정합니다.                                                                                                                                                                                                                                                                        |
| -G, --groups _GROUP1_[,GROUP2...] | 보조 그룹들을 설정합니다. 각 그룹은 공백없이 ,로 구분됩니다.                                                                                                                                                                                                                                                                               |


### 기타 지정 옵션


| 주요 옵션                      | 내용                                                                                                                                |
| ------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------- |
| -p, --password _password_      | 패스워드 지정                                                                                                                       |
| -c, --comment _comment_        | 코맨트 지정                                                                                                                         |
| -e, --expiredate _EXPIRE-DATE_ | 만기날짜 지정, 포맷은 YYYY-MM-DD.<br/> empty string은 no expire를 의미.                                                             |
| -f _EXPIRE-DATE_               | 만기된 후 계정이 **영원히(permanetly)** 비활성화 하기까지의 일 수 지정. <br/> 0은 만기되자마자 비활성화. <br/> -1 은 기능 비활성화. |


### default 관련 옵션

`useradd -D` 를 사용하면 `/etc/default/useradd`에 설정된 설정 디폴트 값을 표시합니다.

뒤에 옵션과 값을 추가하면 디폴트 값을 업데이트하고 `/etc/default/useradd` 파일을 수정합니다.


| 주요 옵션         | 내용                                                                        |
| ----------------- | --------------------------------------------------------------------------- |
| -b, --base-dir    | 기본 디렉토리 경로. 필드는 HOME                                             |
| -g, --gid         | 기본 그룹. 필드는 GROUP.                                                    |
| -s, --shell       | 기본 쉘 지정 필드는 SHELL.                                                  |
| -e, --expireddate | 계정 비밀번호가 만료될 날짜를 지정합니다. 필드는 EXPIRE.                    |
| -f, --inactive    | expired 되면 몇일 뒤에 계정을 사용할 수 없게할지 정합니다. 필드는 INACTIVE. |



## Reference

[die.net, useradd 매뉴얼](https://linux.die.net/man/8/useradd)