# 과제 요구사항
- **Choose the port and host of each ’server’**
- **Setup the server_names or not.**

위의 두가지 config는 이렇게 설정한다.
```nginx

server {

	listen       80;

	server_name  domain1.com www.domain1.com;

}

```
---

• **The first server for a host:port will be the default for this host:port (that means it will answer to all the requests that don’t belong to an other server).**
해당 host와 port에 대한 첫번째 서버가 (여러 서버가 같은 host와 port에 몰릴 수 있음)
client의 접속에 대한 첫번째 기본값이 된다. (즉, 만약 다른 서버가 해당 host 와 port를 listen하고 있지 않다면 첫번째 서버가 응답한다.)

---

- **Setup default error pages.**
```nginx

error_page 404 /custom_404.html;

error_page 500 502 503 504 /custom_50x.html;

```

이렇게 각 에러에 대한 페이지를 설정할  수 있고 만약 설정하지 않은 에러에 대해서는 기본적으로 우리의 에러페이지를 보여준다. 

---
- **Limit client body size** 
```nginx

# 클라이언트 요청 본문 크기 제한 설정 (예: 10MB 제한)

client_max_body_size 10M;

```

1. 클라이언트가 설정한 제한을 초과하는 크기의 본문을 가진 요청을 보낼 때, Nginx는 요청을 거부하고 클라이언트에게 오류 응답을 반환합니다. 보통 413 Payload Too Large 오류 코드와 함께 클라이언트에게 오류 페이지를 보여줍니다.
[`413 Payload Too Large`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/413)

Request entity is larger than limits defined by server. The server might close the connection or return an `Retry-After` header field.
- proxy.conf에 client_max_body_size가 왜 있는지? 에 대한 GPT의 답변. 
```

Nginx 설정 파일에서 `client_max_body_size`를 정의하는 위치에는 다음과 같은 이유로 proxy.conf 파일에 정의하는 것이 일반적입니다:

​

1. 모듈 분리: Nginx는 모듈별로 설정을 분리하고 모듈 간에 설정 파일을 공유할 수 있도록 설계되어 있습니다. `client_max_body_size` 디렉티브는 HTTP 모듈과 관련이 있으므로 일반적으로 `http` 블록 내에 정의됩니다. 그러나 프록시 서버를 사용하는 경우에도 클라이언트 요청에 대한 body 크기 제한을 설정해야 할 수 있습니다. 따라서 이러한 설정을 분리된 파일로 관리하는 것이 모듈화와 가독성을 향상시키는 방법입니다.

2. 설정 관리 용이성: 대규모 또는 복잡한 설정을 관리할 때, 설정을 분리하고 각 모듈 또는 기능에 관련된 설정 파일을 만드는 것이 유지 관리를 더 쉽게 만듭니다. proxy.conf와 같은 파일을 만들고 관련된 설정을 거기에 넣으면 필요한 설정을 빠르게 찾을 수 있고, 변경 및 관리가 용이해집니다.

3. 재사용성: `client_max_body_size`와 같은 설정을 proxy.conf와 같은 파일에 정의하면 여러 서버 블록에서 동일한 설정을 재사용할 수 있습니다. 이렇게 하면 설정 중복을 피하고 일관성을 유지할 수 있습니다.

4. 유지보수 및 협업: 여러 사람이 Nginx 설정을 관리하고 협업하는 경우, 각 모듈 또는 기능에 대한 설정을 분리된 파일로 유지하면 서로의 설정을 간섭하지 않고 독립적으로 작업할 수 있습니다.

​

그러므로 proxy.conf와 같은 별도의 설정 파일을 사용하여 프록시와 관련된 설정을 분리하는 것은 일반적인 관례이며, 복잡한 설정을 관리하기 위한 좋은 방법입니다.

```

# 요구사항​
- 경로 설정에 필요한 규칙/구성은 다음과 같습니다 (주의: 경로에서 정규식은 사용되지 않습니다):
    - 경로에 허용되는 HTTP 메서드 목록을 정의합니다.
    - HTTP 리디렉션을 정의합니다.
    - 파일을 검색할 디렉터리 또는 파일을 정의합니다 (예: url /kapouet이 /tmp/www에 루팅된 경우, url /kapouet/pouic/toto/pouet은 /tmp/www/pouic/toto/pouet입니다).
    - 디렉터리 목록 기능을 켜거나 끕니다.
    - 요청이 디렉터리인 경우 응답할 기본 파일을 설정합니다.
---

## Define a list of accepted HTTP methods for the route.

  (경로에 허용되는 HTTP 메서드 목록을 정의합니다.)
```nginx

limit_except GET POST {

    deny all;

}

```
```nginx

deny GET

allow POST

```
```bnf

<limit-except-block> ::= 'limit_except' <methods> '{' <deny-directive> '}'

<methods> ::= <method> | <method> <methods>

<method> ::= 'GET' | 'POST' | 'PUT' | 'DELETE' | 'HEAD' | 'OPTIONS' | 'PATCH' | 'TRACE' | 'CONNECT'

<deny-directive> ::= 'deny' 'all' ';'

```
- `GET`: 리소스를 가져오는 데 사용됩니다.
- `POST`: 서버에 데이터를 제출하는 데 사용됩니다.
- `PUT`: 서버의 리소스를 생성하거나 교체하는 데 사용됩니다.
- `DELETE`: 서버의 리소스를 삭제하는 데 사용됩니다.
- `HEAD`: 리소스의 헤더만 가져오는 데 사용됩니다.
- `OPTIONS`: 리소스가 지원하는 메소드를 알아내는 데 사용됩니다.
- `PATCH`: 서버의 리소스를 부분적으로 수정하는 데 사용됩니다.
- `TRACE`: 메시지가 프록시를 거쳐 이동하는 경로를 추적하는 데 사용됩니다.
- `CONNECT`: 프록시에 대한 터널 연결을 수립하는 데 사용됩니다.
---
## Define a HTTP redirection.

  (HTTP 리디렉션을 정의합니다.)
- **return 지시어**: `return` 지시어를 사용하여 클라이언트에게 특정 상태 코드와 함께 리다이렉션을 제공할 수 있습니다.
- 예제
```nginx

location = /old-location {

    return 301 /new-location;

}

​

```

- 위의 예제에서 `/old-location`으로의 요청은 301 상태 코드(영구 이동)와 함께 `/new-location`으로 리다이렉트됩니다.
```bnf

<return-directive> ::= 'return' <status-code> <uri> ';'

<status-code> ::= <number>

<number> ::= [0-9]+

```


참조 : [Module ngx_http_rewrite_module (nginx.org)](http://nginx.org/en/docs/http/ngx_http_rewrite_module.html#return)

---

## Define a directory or a file from where the file should be searched (파일을 검색할 디렉터리 또는 파일을 정의합니다)

1. **root 지시어**:
    `root` 지시어는 서버 블록 또는 위치 블록 내에서 요청된 URI의 프리픽스로 사용할 파일 시스템 경로를 정의합니다. `root` 지시어를 사용하면 요청된 URL의 경로가 `root` 지시어에 지정된 경로에 추가되어 파일 시스템에서 파일 또는 디렉터리를 찾습니다.
    예제:
    ```nginx

    location /kapouet {

        root /tmp/www;

    }

    ```

    위의 예제에서, URL `/kapouet/pouic/toto/pouet`는 파일 시스템 경로 `/tmp/www/kapouet/pouic/toto/pouet`로 매핑됩니다.

2. **alias 지시어**:
    `alias` 지시어는 `root`와 유사하지만, URL의 일부를 대체하며 지정된 파일 시스템 경로로 매핑합니다.
    예제:

    ```nginx

    location /kapouet {

        alias /tmp/www;

    }

    ```
    위의 예제에서, URL `/kapouet/pouic/toto/pouet`는 파일 시스템 경로 `/tmp/www/pouic/toto/pouet`로 매핑됩니다.
참조 문서:
- [`root` 지시어 문서](http://nginx.org/en/docs/http/ngx_http_core_module.html#root)
- [`alias` 지시어 문서](http://nginx.org/en/docs/http/ngx_http_core_module.html#alias)

---

## Turn on or off directory listing.

(디렉터리 목록 기능을 켜거나 끕니다.)
1. **autoindex 지시어**:
    `autoindex` 지시어는 디렉터리의 콘텐츠를 나열할지 여부를 제어합니다.
    예제:

    ```nginx

    location /some-directory {

        autoindex on;

    }

    ```
   위의 예제에서 `/some-directory`에 대한 요청은 해당 디렉터리의 콘텐츠를 나열합니다.
    ```nginx

    location /some-directory {

        autoindex off;

    }

    ```

이 예제에서 `/some-directory`에 대한 요청은 디렉터리의 콘텐츠를 나열하지 않습니다.
참조 문서:
- [`autoindex` 지시어 문서](http://nginx.org/en/docs/http/ngx_http_autoindex_module.html#autoindex)
`autoindex` 지시어를 사용하면 클라이언트에게 디렉터리의 콘텐츠를 표시할지 또는 숨길지를 간단하게 제어할 수 있습니다.


> [!NOTE] Index vs autoindex

> index 와 autoindex는 함께 사용할 수 있다. 

> index 가 있다면 index.html을 리턴

> 없다면 autoindex 를 보여준다. 

​

---

​

## Set a default file to answer if the request is a directory.

(요청이 디렉터리인 경우 응답할 기본 파일을 설정합니다.)
1. **index 지시어**:
    `index` 지시어는 디렉터리에 대한 요청이 있을 때 반환할 기본 파일을 지정합니다.
    예제:

    ```nginx

location /some-directory {

	index index.html;

}

    ```
    위의 예제에서 `/some-directory`에 대한 요청은 `index.html` 파일을 반환합니다, 만약 `index.html` 파일이 해당 디렉터리에 존재한다면.

참조 문서:
- [`index` 지시어 문서](http://nginx.org/en/docs/http/ngx_http_index_module.html#index)
`index` 지시어를 사용하면 클라이언트가 디렉터리를 요청할 때 어떤 파일을 기본으로 제공할지 설정할 수 있습니다. 여러 파일을 지정할 수도 있으며, 이 경우 Nginx는 지정된 파일 목록에서 차례대로 첫 번째로 발견되는 파일을 반환합니다.

---

# 이외에 구현에 필요한 config들

> 필수적인 directive와 옵션인 directive를 구분할 것. 

1. `include conf/mime.types;`MIME 타입 설정 파일을 포함시킵니다. 이 파일에는 MIME 타입과 확장자 간의 매핑이 정의되어 있습니다.

2. `include /etc/nginx/proxy.conf;`: 외부 프록시 설정 파일을 포함시킵니다. 이 파일에는 프록시 관련 설정이 들어갑니다.

3. server directives

4. location directives

​

log 파일을 생성하지 않고 log 만 출력하도록 설정. 

1. 파일 생성 log X

2. 파일 제거 log O

3. 파일 생성 log O **(채택)**

	1. 파일에는 모든 req, res를 보여주고 log에는 간소화된 버전으로 출력 (*이 방향으로 구현할 것*)

​

아래는 access.log의 format 형식.

```

Nginx의 `access.log` 파일에는 웹 서버에 들어온 요청과 해당 요청에 대한 응답에 관한 로그 정보가 기록됩니다. 아래는 일반적인 Nginx `access.log` 파일의 예시입니다:

​

arduinoCopy code

​

`192.168.1.100 - - [24/Sep/2023:12:34:56 +0000] "GET /example/page.html HTTP/1.1" 200 1234 "http://example.com/referrer" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/94.0.4606.54 Safari/537.36"`

​

이 로그 항목은 다음 정보를 포함합니다:

​

1. `192.168.1.100`: 클라이언트의 IP 주소 또는 호스트명.

2. `- -`: 다음 두 필드는 보통 사용되지 않으며, 웹 서버에서 추가 정보를 로깅하기 위해 구성될 수 있습니다. 첫 번째 필드는 원격 사용자의 식별을 나타내며, 두 번째 필드는 인증된 사용자를 나타냅니다.

3. `[24/Sep/2023:12:34:56 +0000]`: 요청이 발생한 일시를 나타내며 일반적으로 `[날짜:시간 +시간대]` 형식입니다.

4. `"GET /example/page.html HTTP/1.1"`: HTTP 요청 메서드, 요청한 URL 경로, 및 사용된 HTTP 프로토콜 버전.

5. `200`: HTTP 응답 상태 코드. 여기서는 200으로 성공적인 요청을 나타냅니다.

6. `1234`: 응답으로 전송된 바이트 수 또는 응답 본문의 크기.

7. `"http://example.com/referrer"`: Referer 헤더로 전송된 이전 페이지의 URL. 클라이언트에서 이 페이지로 연결된 경우 이전 페이지의 URL이 여기에 나타납니다.

8. `"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/94.0.4606.54 Safari/537.36"`: 사용자 에이전트(User Agent) 문자열. 클라이언트 브라우저와 운영 체제 정보를 포함합니다.

​

실제로 Nginx의 `access.log` 파일은 이 예시보다 훨씬 더 많은 로그 항목을 포함하고 있을 것이며, 더 복잡한 웹 서버 환경에서는 더 많은 정보가 기록됩니다.로그 형식 및 정보는 Nginx 설정 파일에서 설정을 통해 조정할 수 있으므로 실제로 기록되는 정보는 서버 설정에 따라 다를 수 있습니다.

```


- Execute CGI based on certain file extension (for example .php).

```nginx

server {

	location /.php {  

		cgi_pass "path/to/cgi/dir";  

	}

}

```

- Make the route able to accept uploaded files and configure where they should be saved.

```nginx

server {  

	location /upload {
	
		directory "path/to/upload/dir";

	}

}  

```

> 나머지는 CGI 구현시 요구사항을 적어 놓음. 

## BNF

```bnf

<nginx-config> ::= <server-block>

​

<server-block> ::= 'server' '{' <server-content> '}'

​

<server-content> ::= <listen-directive> <server-name-directive> <location-block> <location-block>

​

<listen-directive> ::= 'listen' <port> ';'

​

<server-name-directive> ::= 'server_name' <domain> ';'

​

<location-block> ::= 'location' <uri> '{' <location-content> '}'

​

<location-content> ::= <limit-except-block> | <alias-directive> | <autoindex-directive> | <index-directive> | <return-directive>

​

<limit-except-block> ::= 'limit_except' <methods> '{' <deny-directive> '}'

​

<alias-directive> ::= 'alias' <path> ';'

​

<autoindex-directive> ::= 'autoindex' ('on' | 'off') ';'

​

<index-directive> ::= 'index' <file> ';'

​

<return-directive> ::= 'return' <status-code> <uri> ';'

​

<port> ::= <number>

​

<domain> ::= <string>

​

<uri> ::= <string>

​

<methods> ::= <method> | <method> <methods>

​

<method> ::= 'GET' | 'POST'

​

<deny-directive> ::= 'deny' 'all' ';'

​

<path> ::= <string>

​

<file> ::= <string>

​

<status-code> ::= <number>

​

<number> ::= [0-9]+

​

<string> ::= '"' .* '"'

```
