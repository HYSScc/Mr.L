1.安装更新

\#git clone [https:\/\/github.com\/wg\/wrk.git](https://github.com/wg/wrk.git)

2.安装部署

\#cd wrk

\[root@wrk\]\# cat INSTALL

 For example to use the system version of both libraries on Linux:

 make WITH\_LUAJIT=\/usr WITH\_OPENSSL=\/usr

 Or to use the Homebrew version of OpenSSL on Mac OS X:

 make WITH\_OPENSSL=\/usr\/local\/opt\/openssl



\#make WITH\_LUAJIT=\/xxxxx WITH\_OPENSSL=\/xxxxxx

3.测试

\#.\/wrk -t5 -c500 -d1m [http:\/\/127.0.0.1:8080\/index.html](http://127.0.0.1:8080/index.html)

```
  Running 30s test @ http://127.0.0.1:8080/index.html
    12 threads and 400 connections
    Thread Stats   Avg      Stdev     Max   +/- Stdev
      Latency   635.91us    0.89ms  12.92ms   93.69%
      Req/Sec    56.20k     8.07k   62.00k    86.54%
    22464657 requests in 30.00s, 17.76GB read
  Requests/sec: 748868.53
  Transfer/sec:    606.33MB
```

