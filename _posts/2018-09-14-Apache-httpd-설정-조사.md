---
layout: post
title: Apache-httpd 설정 정리
date: 2018-09-14 10:43:00 -0600
category: 개발
---

## Apache Web Server의 서버 설정 파일(httpd.conf)의 내용 분석

- ServerRoot
    - Web Server가 설치된 디렉토리 경로
    - 예
      ```
      ServerRoot "D:/Apache/httpd-2.2.31-win64/Apache2"
      ```

- Listen
    - 서버가 요청 받을 IP주소와 port설정
        - 2개이상의 포트 설정 예
          ```
          Listen 80
          Listen 8000
          ```
        - 2개의 지정된 인터페이스와 포트 번호에서 연결 허용하기
          ```
          Listen 192.170.2.1:80
          Listen 192.170.2.5:8000
          ```
        - IPv6 주소로 하려면 []괄호를 감싸줘야 한다.
          ```
          Listen [2001:db8::a00:20ff:fea7:ccea]:80
          ```
        - 비표준 포트에서 연결하고자 할 때. (https기본 포트 : 443)
          ```
          Listen 192.170.2.1:8443 https
          ```
- LoadModule
    - 목적파일이나 라이브러리를 읽어들이고, 사용가능한 모듈 목록에 추가한다
      ```
      LoadModule actions_module modules/mod_actions.so
      ```
- `<IfModule>`
    - 특정 모듈의 존재 여부에 따라 지시문을 포함한다.
      ```
      <IfModule !mpm_netware_module>
        <IfModule !mpm_winnt_module>
            User daemon
            Group daemon
        </IfModule>
      </IfModule>
      ```
      위의 예에서 mpm_netware_module와 mpm_winnt_module이 둘 다 없는 경우 해당 지시문이 포함된다.
        - daemon 이란 백그라운드로 실행되면서, 사용자의 인터페이스(tty)가 없는 프로그램을 말한다.
- ServerAdmin
    - 서버가 클라이언트에게 보낸 메세지에 포함되는 이메일 주소
- ServerName
    - 서버가 사용할 Hostname과 port
    - DNS가 등록되어 있지 않으면 IP주소를 입력한다.
      ```
      ServerName localhost:80
      ```
- DocumentRoot
    - 웹에서 볼 수 있는 main document tree의 경로
      ```
      DocumentRoot /usr/web
      ```
      위와 같이 설정하면 http://www.my.host.com/index.html 을 요청했을 때, /usr/web/index.html을 응답한다.
- `<Directory>`
    - 지정한 디렉토리이하의 모든 웹문서들에 대하여 어떤 서비스와 기능을 허용/거부 할 것인지를 설정하는 매우 중요한 지시자이다. 현재 루트(/) 디렉토리에 대해 심볼릭링크를 허용하고 .htaccess 파일의 사용을 거부한다. <Directory>지시자의 설정은 개인에 따라 다르니 각자 목적에 맞게 설정해야 한다.

      ```
      <Directory />
        Options FollowSymLinks
        AllowOverride None
        Order deny,allow
        Deny from all
      </Directory>
      ```
    
      - Options
      
        지정한 디렉토리이하에 모든 파일과 디렉토리들에 적용할 접근제어를 설정한다. 디렉토리목록을 보여줄지, CGI를 허용할 것인지 등등의 것들의 설정을 여기서 하게 된다.

        가상호스트를 사용하는 경우나 하위디렉토리에서 위와 같은 루트(상위 디렉토리)에 대한 설정이 어떻게 적용되어 있던간에 가상호스트 안이나 하위디렉토리에 다시 Options 값을 지정할 수 있다. 이때 상위디렉토리의 다른 옵션은 변경하지 않고 특정 옵션만 제거하거나 추가할때 + 나 - 를 Options 값 앞에 붙여 사용하기도 한다.

        Options FollowSymLinks는 상위 디렉토리 설정에서 허가된 FollowSymLinks를 제거하게 되며, Options +Indexes는 상위 디렉토리 설정에 없는 Indexes 설정을 추가한다.

        | 종류           	| 설명                                                                                                                                                                                                                                                                                                                                	|
        |----------------	|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------	|
        | None           	| 모든 허용을 하지 않는다. 즉 None 설정으로 이외의 다른 설정들은 모두 무시한다                                                                                                                                                                                                                                                        	|
        | All            	| MultiViews를 제외한 모든 옵션설정을 허용한다. Options 값이 공백일때도 All과 같다.(Options (없음))                                                                                                                                                                                                                                   	|
        | Indexes        	| 웹서버의 디렉토리 접근시에 DirectoryIndex에서 지정한 파일(index.html등)이 존재하지 않을 경우에 디렉토리내의 파일목록리스트를 웹브라우저로 보여준다. 서버보안을 위해 사용하지 않는 것이 좋다                                                                                                                                         	|
        | Includes       	| SSI 사용을 허용하는 설정이다. 단 mod_include.c 라는 모듈이 필요하며 기본적으로 로드되어 있다.                                                                                                                                                                                                                                       	|
        | IncludesNOEXEC 	| SSI 사용은 허용되지만 #exec 사용과 #include는 허용되지 않는다. 즉 SSI를 사용하면서 시스템에 위험한 SSI의 실행태그는 허용하지 않겠다는 설정이다.                                                                                                                                                                                     	|
        | FollowSymlinks 	| 심볼릭 링크를 허용한다. 이 옵션을 지정하면 웹브라우저에서 링크파일의 경로까지도 확인 할 수 있게된다. 보안상 이 값은 설정하지 않는 것이 좋다.                                                                                                                                                                                        	|
        | ExecCGI        	| perl등과 같은 CGI 실행을 허용하기 위한 설정이다. 원래 아파치에서 CGI 사용은 ScriptAlias로 지정된 위치에서 사용하는 것이 기본이다. 하지만 ScriptAlias가 지정되지 않은 디렉토리에 이 옵션이 지정되어있다면 지정된 디렉토리내에서는 CGI 사용이 허용된다. 물론 이 경우에도 "AddHandler cgi-script" 지시자에서 정의한 확장자만 유효하다. 	|
        | MultiViews     	| 웹브라우저의 요청에 따라 적절한 페이지로 보여준다. 웹브라우저의 종류나 웹문서의 종류에 따라서 가장 적합한 페이지를 보여줄 수 있도록하는 설정이다.                                                                                                                                                                                   	|

      - AllowOverride
        
        AllowOverride 지시자는 어떻게 접근을 허락할 것인가에 대한 설정이다. 특정 디렉토리에 대한 방문자들의 접근방식을 어떤방식으로 인증하여 허용할 것인가의 문제라고 할 수 있다. AllowOverride에서 설정하는 값들은 중복해서 설정될 수 있으며 그때마다 가장 최근에 설정된 값잉 항상 우선적용된다.

        | 종류       	| 설명                                                                                                                                                                                                                                                                                                                                                                       	|
        |------------	|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------	|
        | None       	| 이 값이 설정되면 AccessFileName에 지정된 파일을 액세스 인증파일로 인식하지 않는다.즉 AccessFileName의 값이 대부분 .htaccess 이므로 이를 무시하게 된다. 아주 제한적인 접근만을 허용할때 사용하는 값이다.                                                                                                                                                                    	|
        | All        	| 이전의 인증방식에 대하여 새로운 접근인증방식을 우선적용하도록 Override를 허용한다.                                                                                                                                                                                                                                                                                         	|
        | AuthConfig 	| AccessFileName 지시자에 명시한 파일에 대하여 AuthDBMGroupFile, AuthDBMUserFile, AuthGroupFile, AuthName, AuthType, AuthUserFile, require 등과 같은 클라이언트 인증지시자의 사용을 허용한다. 즉 htpasswd 유틸리티를 이용하여 특정디렉토리의 접근은 AccessFileName에 명시한 파일(.htaccess)로 제어하고자 할 때에 해당 디렉토리내에 이 값을 주로 사용한다.(디렉토리 인증설정) 	|
        | FileInfo   	| AccessFileName 지시자에 명시한 파일에 대하여 AddEncoding, AddLanguage, AddType, DefaultType, ErrorDocument, LanguagePriority 등과 같은 문서유형을 제어하는 지시자 사용을 허용한다.                                                                                                                                                                                         	|
        | Indexes    	| AccessFileName 지시자에 명시한 파일에 대하여 AddDescription, AddIcon, AddIconByEncoding, AddIconByType, DefaultIcon, DirectoryIndex, FancyIndexing, HeaderName, IndexIgnore, IndexOptions, ReadmeName 등과 같은 디렉토리 Indexing을 제어하는 지시자 사용을 허용한다.                                                                                                       	|
        | Options    	| AccessFileName 지시자에 명시한 파일에 대하여 Options 그리고 XBitHack등과 같은 특정 디렉토리옵션을 제어하는 지시자 사용을 허용한다.                                                                                                                                                                                                                                         	|
        | Limit      	| AccessFileName 지시자에 명시한 파일에 대하여 allow, deny, 그리고 order 등과 같은 호스트 접근을 제어하는 지시자 사요을 허용한다.                                                                                                                                                                                                                                            	|

      - Order  
        order 항목은 접근을 통제하는 순서를 나타낸다. allow와 deny 순으로 지정하면 allow 기능을 먼저 수행하고, deny기능을 나중에 수행하라는 의미다.

        - deny, allow – deny 지시자 부터 검사하고 allow 지시자를 검사  
        - allow, deny – allow 지시자 부터 검사하고 deny 지시자를 검사  
        - mutual-failure – allow목록에 없는 모든 host에게 접속을 거부  
        - allow from 항목 다음에 쓸수 있는 항목은 아래와 같다.  

      - Allow 와 Deny  
        Allow from 항목 다음에 쓸 수 있는 항목은 아래와 같다.
        - 사용 가능한 주소는 도메인 네임
        - 호스트 이름 주소
        - 호스트 ip 주소
        - ip 주소의 앞부분 3바이트
        - 모든 주소일 경우 all을 사용한다
    
        deny from 항목은 allow from과 반대다.
- `<FilesMatch>`
    - 정규식을 사용하여 해당하는 파일 이름에 대한 지시문을 포함한다.
      ```
      <FilesMatch "^\.ht">
      ```
- ErrorLog
    - 서버가 error 로그를 남기는 위치
- LogLevel
    - 로깅을 할 레벨을 지정함.
    - 종류 : debug, info, notice, warn, error, crit, alert, emerg.
- ScriptAlias
    - URL을 특정 파일시스템 장소로 대응하고 대상이 CGI 스크립트라고 알린다
    - CGI?
        - CGI(Common Gate Interface)란 서버와 외부 스크립트 또는 프로그램과 상호작용할 때 이루어지는 입출력을 정의한 표준이며, 이 표준에 맞추어 만들어진 것이 CGI 스크립트 또는 CGI 프로그램며 CGI 프로그램은 어떤 프로그래밍 언어로도 만들 수 있다.
- DefaultType
    - 서버가 다른 형식으로 유형을 판별 할 수없는 경우 전송할 MIME 내용 유형
    - MIME?
        - (Multipurpose Internet Mail Extensions)는 전자 우편을 위한 인터넷 표준 포맷이다

#### 출처
[http://httpd.apache.org/docs/2.2/mod/directives.html](http://httpd.apache.org/docs/2.2/mod/directives.html)  
[http://win100.tistory.com/120](http://win100.tistory.com/120)  
[http://webdir.tistory.com/178](http://webdir.tistory.com/178)  
[http://sfeg.tistory.com/196](http://sfeg.tistory.com/196)  
