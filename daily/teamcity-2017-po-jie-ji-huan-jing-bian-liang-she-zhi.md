# TeamCity破解及环境变量

---

破解（很老的一个版本，但是测试发现适合2017,2018也应该适合）：[https://pan.baidu.com/s/17KWRRMNvGBJwJzPQwjQrdg](https://pan.baidu.com/s/17KWRRMNvGBJwJzPQwjQrdg)

另外agent自定义环境变量，在/etc/profile和.bashrc 都加了，但是却识别不了

结果应该是在 /buildAgent/conf/buildAgent.properties中增加，在系统中增加可能识别也可能不识别（暂时没去深究）

