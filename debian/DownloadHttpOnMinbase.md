Example of downloading a file via HTTP on Debian minbase (without curl/wget/…)
==============================================================================

Script:
```
cat > /tmp/http-cmd.aptscript <<EOF
600 URI Acquire
URI: http://mirror.mephi.ru/other/2015/fizikishut.pdf
Filename: /tmp/test.pdf


EOF

(cat /tmp/cmd; sleep 1) | /usr/lib/apt/methods/http
```

Result:
```
$ cat > /tmp/http-cmd.aptscript <<EOF
> 600 URI Acquire
> URI: http://mirror.mephi.ru/other/2015/fizikishut.pdf
> Filename: /tmp/test.pdf
> 
> 
> EOF

$ (cat /tmp/cmd; sleep 1) | /usr/lib/apt/methods/http
100 Capabilities
Version: 1.2
Pipeline: true
Send-Config: true

102 Status
URI: http://mirror.mephi.ru/other/2015/fizikishut.pdf
Message: Connecting to mirror.mephi.ru

102 Status
URI: http://mirror.mephi.ru/other/2015/fizikishut.pdf
Message: Connecting to mirror.mephi.ru (10.50.0.93)

102 Status
URI: http://mirror.mephi.ru/other/2015/fizikishut.pdf
Message: Waiting for headers

200 URI Start
URI: http://mirror.mephi.ru/other/2015/fizikishut.pdf
Size: 1513623
Last-Modified: Fri, 10 Jun 2016 16:22:31 GMT
Resume-Point: 1513623

201 URI Done
URI: http://mirror.mephi.ru/other/2015/fizikishut.pdf
Filename: /tmp/test.pdf
Size: 1513623
Last-Modified: Fri, 10 Jun 2016 16:22:31 GMT
MD5-Hash: 66ae82af5a4a80b79311b476a10430a2
MD5Sum-Hash: 66ae82af5a4a80b79311b476a10430a2
SHA1-Hash: 8265cb05cff99d74f5edf73bcff94ce983915bb5
SHA256-Hash: 1b93e29f759fb7f84d780dd532b5ab6f3c56945938d6fd5ba043909d31d79a46
SHA512-Hash: 893c48ae05630fdb895a6a2c89cfdacdb2d7bfc81e60c748c3a511d49ab5cb81c3bf5192efc7ae8d30f0445bb5cc753b16a7891aca77ffe8f7dea157f241e717
Resume-Point: 1513623

$ ls -ld /tmp/test.pdf 
-rw-r--r-- 1 xaionaro xaionaro 1513623 Jun 10 19:22 /tmp/test.pdf
```