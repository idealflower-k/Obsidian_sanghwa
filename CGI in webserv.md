```
GET / HTTP/1.1
Host: 127.0.0.1:4240
Upgrade-Insecure-Requests: 1
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/15.6.1 Safari/605.1.15
Accept-Language: en-us
Accept-Encoding: gzip, deflate
Connection: keep-alive
```

1. location으로 설정된 경로의 다음 부분은 ?전까지 파일의 경로로 해석한다.
2. ? 뒤에 부터 끝까지 QUERY_STRING으로 해석한다.
3. PATH_INFO: cgi파일의 전체 경로로 설정해준다.