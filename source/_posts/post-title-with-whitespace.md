---
title: (Spring Cloud微服务实战-书中之坑)分布式服务跟踪：Spring Cloud Sleuth 快速入门
date: 2019-05-27 11:47:30
tags:
---

按照书中的快速入门案例，在pom.xml文件中加入eureka和ribbon依赖

    <artifactId>spring-cloud-starter-eureka</artifactId>
    <artifactId>spring-cloud-starter-ribbon</artifactId>
分别实现trace-1和trace-2接口，发送POST请求出错：
<br>

<html><body><h1>Whitelabel Error Page</h1><p>This application has no explicit mapping for /error, so you are seeing this as a fallback.</p><div id='created'>Wed May 15 20:39:45 CST 2019</div><div>There was an unexpected error (type=Method Not Allowed, status=405).</div><div>Request method &#39;POST&#39; not supported</div></body></html>
<br>
解决办法：
依赖改为：<br>
<artifactId>spring-cloud-starter-netflix-eureka-server</artifactId><br>
<artifactId>spring-cloud-starter-netflix-ribbon</artifactId>
<br><br>
只需要把trace1的RequestMethod.GET改为POST即可，trace2不需要改<br>
@RequestMapping(value = "/trace-1", method = RequestMethod.POST)<br>
    public String trace(){<br>
        System.out.println("===call trace-1===");<br>
        return restTemplate().getForEntity("http://TRACE-2/trace-2", String.class).getBody();
    }<br><br>
 @RequestMapping(value = "/trace-2", method = RequestMethod.GET)<br>
    public String trace(){<br>
        System.out.println("===<call trace-2>===");<br>
        return "Trace";<br>
    }<br>
    版本问题是个令人头疼的问题，书中也有各种坑ヽ(ー_ー)ノ
