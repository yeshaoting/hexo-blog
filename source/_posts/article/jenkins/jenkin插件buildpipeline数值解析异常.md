
categories:
  - jenkins

tags:
  - jenkins
  - 问题

title: jenkins build-pipeline-plugin 数值解析异常
---

[TOC]

jenkins version: 1.574

## 异常描述
遇到的异常堆栈内容如下：
``` java
A problem occurred while processing the request. Please check our bug tracker to see if a similar problem has already been reported. If it is already reported, please vote and put a comment on it to let us gauge the impact of the problem. If you think this is a new issue, please file a new issue. When you file an issue, make sure to add the entire stack trace, along with the version of Jenkins and relevant plugins. The users list might be also useful in understanding what has happened.

Stack trace

javax.servlet.ServletException: org.apache.commons.jelly.JellyTagException: jar:file:/var/cache/jenkins/war/WEB-INF/lib/jenkins-core-1.574.jar!/hudson/model/View/index.jelly:42:43: <st:include> org.apache.commons.jelly.JellyTagException: jar:file:/var/cache/jenkins/war/WEB-INF/lib/jenkins-core-1.574.jar!/lib/hudson/projectView.jelly:66:22: <d:invokeBody> null
	at org.kohsuke.stapler.jelly.JellyClassTearOff.serveIndexJelly(JellyClassTearOff.java:117)
	at org.kohsuke.stapler.jelly.JellyFacet.handleIndexRequest(JellyFacet.java:127)
	at org.kohsuke.stapler.Stapler.tryInvoke(Stapler.java:717)
	at org.kohsuke.stapler.Stapler.invoke(Stapler.java:858)
	at org.kohsuke.stapler.Stapler.tryInvoke(Stapler.java:795)
	at org.kohsuke.stapler.Stapler.invoke(Stapler.java:858)
	at org.kohsuke.stapler.Stapler.invoke(Stapler.java:631)
	at org.kohsuke.stapler.Stapler.service(Stapler.java:225)
	at javax.servlet.http.HttpServlet.service(HttpServlet.java:848)
	at org.eclipse.jetty.servlet.ServletHolder.handle(ServletHolder.java:686)
	at org.eclipse.jetty.servlet.ServletHandler$CachedChain.doFilter(ServletHandler.java:1494)
	at hudson.util.PluginServletFilter$1.doFilter(PluginServletFilter.java:96)
	at hudson.util.PluginServletFilter.doFilter(PluginServletFilter.java:88)
	at org.eclipse.jetty.servlet.ServletHandler$CachedChain.doFilter(ServletHandler.java:1482)
	at hudson.security.csrf.CrumbFilter.doFilter(CrumbFilter.java:48)
	at org.eclipse.jetty.servlet.ServletHandler$CachedChain.doFilter(ServletHandler.java:1482)
	at hudson.security.ChainedServletFilter$1.doFilter(ChainedServletFilter.java:84)
	at hudson.security.UnwrapSecurityExceptionFilter.doFilter(UnwrapSecurityExceptionFilter.java:51)
	at hudson.security.ChainedServletFilter$1.doFilter(ChainedServletFilter.java:87)
	at jenkins.security.ExceptionTranslationFilter.doFilter(ExceptionTranslationFilter.java:117)
	at hudson.security.ChainedServletFilter$1.doFilter(ChainedServletFilter.java:87)
	at org.acegisecurity.providers.anonymous.AnonymousProcessingFilter.doFilter(AnonymousProcessingFilter.java:125)
	at hudson.security.ChainedServletFilter$1.doFilter(ChainedServletFilter.java:87)
	at org.acegisecurity.ui.rememberme.RememberMeProcessingFilter.doFilter(RememberMeProcessingFilter.java:142)
	at hudson.security.ChainedServletFilter$1.doFilter(ChainedServletFilter.java:87)
	at org.acegisecurity.ui.AbstractProcessingFilter.doFilter(AbstractProcessingFilter.java:271)
	at hudson.security.ChainedServletFilter$1.doFilter(ChainedServletFilter.java:87)
	at org.acegisecurity.ui.basicauth.BasicProcessingFilter.doFilter(BasicProcessingFilter.java:174)
	at hudson.security.ChainedServletFilter$1.doFilter(ChainedServletFilter.java:87)
	at jenkins.security.ApiTokenFilter.doFilter(ApiTokenFilter.java:79)
	at hudson.security.ChainedServletFilter$1.doFilter(ChainedServletFilter.java:87)
	at org.acegisecurity.context.HttpSessionContextIntegrationFilter.doFilter(HttpSessionContextIntegrationFilter.java:249)
	at hudson.security.HttpSessionContextIntegrationFilter2.doFilter(HttpSessionContextIntegrationFilter2.java:67)
	at hudson.security.ChainedServletFilter$1.doFilter(ChainedServletFilter.java:87)
	at hudson.security.ChainedServletFilter.doFilter(ChainedServletFilter.java:76)
	at hudson.security.HudsonFilter.doFilter(HudsonFilter.java:164)
	at org.eclipse.jetty.servlet.ServletHandler$CachedChain.doFilter(ServletHandler.java:1482)
	at org.kohsuke.stapler.compression.CompressionFilter.doFilter(CompressionFilter.java:46)
	at org.eclipse.jetty.servlet.ServletHandler$CachedChain.doFilter(ServletHandler.java:1482)
	at hudson.util.CharacterEncodingFilter.doFilter(CharacterEncodingFilter.java:81)
	at org.eclipse.jetty.servlet.ServletHandler$CachedChain.doFilter(ServletHandler.java:1474)
	at org.eclipse.jetty.servlet.ServletHandler.doHandle(ServletHandler.java:499)
	at org.eclipse.jetty.server.handler.ScopedHandler.handle(ScopedHandler.java:137)
	at org.eclipse.jetty.security.SecurityHandler.handle(SecurityHandler.java:533)
	at org.eclipse.jetty.server.session.SessionHandler.doHandle(SessionHandler.java:231)
	at org.eclipse.jetty.server.handler.ContextHandler.doHandle(ContextHandler.java:1086)
	at org.eclipse.jetty.servlet.ServletHandler.doScope(ServletHandler.java:428)
	at org.eclipse.jetty.server.session.SessionHandler.doScope(SessionHandler.java:193)
	at org.eclipse.jetty.server.handler.ContextHandler.doScope(ContextHandler.java:1020)
	at org.eclipse.jetty.server.handler.ScopedHandler.handle(ScopedHandler.java:135)
	at org.eclipse.jetty.server.handler.HandlerWrapper.handle(HandlerWrapper.java:116)
	at org.eclipse.jetty.server.Server.handle(Server.java:370)
	at org.eclipse.jetty.server.AbstractHttpConnection.handleRequest(AbstractHttpConnection.java:489)
	at org.eclipse.jetty.server.AbstractHttpConnection.headerComplete(AbstractHttpConnection.java:949)
	at org.eclipse.jetty.server.AbstractHttpConnection$RequestHandler.headerComplete(AbstractHttpConnection.java:1011)
	at org.eclipse.jetty.http.HttpParser.parseNext(HttpParser.java:644)
	at org.eclipse.jetty.http.HttpParser.parseAvailable(HttpParser.java:235)
	at org.eclipse.jetty.server.AsyncHttpConnection.handle(AsyncHttpConnection.java:82)
	at org.eclipse.jetty.io.nio.SelectChannelEndPoint.handle(SelectChannelEndPoint.java:668)
	at org.eclipse.jetty.io.nio.SelectChannelEndPoint$1.run(SelectChannelEndPoint.java:52)
	at winstone.BoundedExecutorService$1.run(BoundedExecutorService.java:77)
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1156)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:626)
	at java.lang.Thread.run(Thread.java:804)
Caused by: org.apache.commons.jelly.JellyTagException: jar:file:/var/cache/jenkins/war/WEB-INF/lib/jenkins-core-1.574.jar!/hudson/model/View/index.jelly:42:43: <st:include> org.apache.commons.jelly.JellyTagException: jar:file:/var/cache/jenkins/war/WEB-INF/lib/jenkins-core-1.574.jar!/lib/hudson/projectView.jelly:66:22: <d:invokeBody> null
	at org.apache.commons.jelly.impl.TagScript.handleException(TagScript.java:726)
	at org.apache.commons.jelly.impl.TagScript.run(TagScript.java:281)
	at org.apache.commons.jelly.impl.ScriptBlock.run(ScriptBlock.java:95)
	at org.kohsuke.stapler.jelly.CallTagLibScript$1.run(CallTagLibScript.java:99)
	at org.apache.commons.jelly.tags.define.InvokeBodyTag.doTag(InvokeBodyTag.java:91)
	at org.apache.commons.jelly.impl.TagScript.run(TagScript.java:269)
	at org.apache.commons.jelly.impl.ScriptBlock.run(ScriptBlock.java:95)
	at org.apache.commons.jelly.tags.core.CoreTagLibrary$1.run(CoreTagLibrary.java:98)
	at org.apache.commons.jelly.impl.ScriptBlock.run(ScriptBlock.java:95)
	at org.apache.commons.jelly.tags.core.CoreTagLibrary$2.run(CoreTagLibrary.java:105)
	at org.kohsuke.stapler.jelly.CallTagLibScript.run(CallTagLibScript.java:120)
	at org.apache.commons.jelly.impl.ScriptBlock.run(ScriptBlock.java:95)
	at org.kohsuke.stapler.jelly.CallTagLibScript$1.run(CallTagLibScript.java:99)
	at org.apache.commons.jelly.tags.define.InvokeBodyTag.doTag(InvokeBodyTag.java:91)
	at org.apache.commons.jelly.impl.TagScript.run(TagScript.java:269)
	at org.apache.commons.jelly.impl.ScriptBlock.run(ScriptBlock.java:95)
	at org.kohsuke.stapler.jelly.ReallyStaticTagLibrary$1.run(ReallyStaticTagLibrary.java:99)
	at org.kohsuke.stapler.jelly.ReallyStaticTagLibrary$1.run(ReallyStaticTagLibrary.java:99)
	at org.apache.commons.jelly.impl.ScriptBlock.run(ScriptBlock.java:95)
	at org.kohsuke.stapler.jelly.ReallyStaticTagLibrary$1.run(ReallyStaticTagLibrary.java:99)
	at org.kohsuke.stapler.jelly.ReallyStaticTagLibrary$1.run(ReallyStaticTagLibrary.java:99)
	at org.apache.commons.jelly.impl.ScriptBlock.run(ScriptBlock.java:95)
	at org.kohsuke.stapler.jelly.ReallyStaticTagLibrary$1.run(ReallyStaticTagLibrary.java:99)
	at org.apache.commons.jelly.impl.ScriptBlock.run(ScriptBlock.java:95)
	at org.kohsuke.stapler.jelly.ReallyStaticTagLibrary$1.run(ReallyStaticTagLibrary.java:99)
	at org.apache.commons.jelly.impl.ScriptBlock.run(ScriptBlock.java:95)
	at org.apache.commons.jelly.tags.core.CoreTagLibrary$2.run(CoreTagLibrary.java:105)
	at org.kohsuke.stapler.jelly.CallTagLibScript.run(CallTagLibScript.java:120)
	at org.kohsuke.stapler.jelly.CompressTag.doTag(CompressTag.java:44)
	at org.apache.commons.jelly.impl.TagScript.run(TagScript.java:269)
	at org.kohsuke.stapler.jelly.JellyViewScript.run(JellyViewScript.java:81)
	at org.kohsuke.stapler.jelly.DefaultScriptInvoker.invokeScript(DefaultScriptInvoker.java:63)
	at org.kohsuke.stapler.jelly.DefaultScriptInvoker.invokeScript(DefaultScriptInvoker.java:53)
	at org.kohsuke.stapler.jelly.JellyClassTearOff.serveIndexJelly(JellyClassTearOff.java:112)
	... 63 more
Caused by: java.lang.RuntimeException: org.apache.commons.jelly.JellyTagException: jar:file:/var/cache/jenkins/war/WEB-INF/lib/jenkins-core-1.574.jar!/lib/hudson/projectView.jelly:66:22: <d:invokeBody> null
	at org.kohsuke.stapler.jelly.groovy.JellyBuilder.doInvokeMethod(JellyBuilder.java:280)
	at org.kohsuke.stapler.jelly.groovy.Namespace$ProxyImpl.invoke(Namespace.java:92)
	at com.sun.proxy.$Proxy56.projectView(Unknown Source)
	at lib.JenkinsTagLib$projectView.call(Unknown Source)
	at hudson.model.View.main.run(main.groovy:14)
	at org.kohsuke.stapler.jelly.groovy.GroovierJellyScript.run(GroovierJellyScript.java:74)
	at org.kohsuke.stapler.jelly.groovy.GroovierJellyScript.run(GroovierJellyScript.java:62)
	at org.kohsuke.stapler.jelly.IncludeTag.doTag(IncludeTag.java:147)
	at org.apache.commons.jelly.impl.TagScript.run(TagScript.java:269)
	... 95 more
Caused by: org.apache.commons.jelly.JellyTagException: jar:file:/var/cache/jenkins/war/WEB-INF/lib/jenkins-core-1.574.jar!/lib/hudson/projectView.jelly:66:22: <d:invokeBody> null
	at org.apache.commons.jelly.impl.TagScript.handleException(TagScript.java:726)
	at org.apache.commons.jelly.impl.TagScript.run(TagScript.java:281)
	at org.apache.commons.jelly.impl.ScriptBlock.run(ScriptBlock.java:95)
	at org.apache.commons.jelly.tags.core.CoreTagLibrary$1.run(CoreTagLibrary.java:98)
	at org.apache.commons.jelly.impl.ScriptBlock.run(ScriptBlock.java:95)
	at org.kohsuke.stapler.jelly.ReallyStaticTagLibrary$1.run(ReallyStaticTagLibrary.java:99)
	at org.apache.commons.jelly.impl.ScriptBlock.run(ScriptBlock.java:95)
	at org.apache.commons.jelly.tags.core.CoreTagLibrary$2.run(CoreTagLibrary.java:105)
	at org.kohsuke.stapler.jelly.CallTagLibScript.run(CallTagLibScript.java:120)
	at org.kohsuke.stapler.jelly.groovy.JellyBuilder.doInvokeMethod(JellyBuilder.java:276)
	... 103 more
Caused by: java.lang.NumberFormatException: null
	at java.lang.Integer.parseInt(Integer.java:465)
	at java.lang.Integer.valueOf(Integer.java:593)
	at au.com.centrumsystems.hudson.plugin.buildpipeline.BuildPipelineView.getBuildPipelineForm(BuildPipelineView.java:400)
	at au.com.centrumsystems.hudson.plugin.buildpipeline.BuildPipelineView.getItems(BuildPipelineView.java:866)
	at au.com.centrumsystems.hudson.plugin.buildpipeline.BuildPipelineView.hasPermission(BuildPipelineView.java:932)
	at hudson.model.ViewGroupMixIn.getViews(ViewGroupMixIn.java:115)
	at jenkins.model.Jenkins.getViews(Jenkins.java:1444)
	at sun.reflect.GeneratedMethodAccessor255.invoke(Unknown Source)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:55)
	at java.lang.reflect.Method.invoke(Method.java:618)
	at org.codehaus.groovy.reflection.CachedMethod.invoke(CachedMethod.java:90)
	at groovy.lang.MetaMethod.doMethodInvoke(MetaMethod.java:233)
	at groovy.lang.MetaClassImpl$GetBeanMethodMetaProperty.getProperty(MetaClassImpl.java:3500)
	at org.codehaus.groovy.runtime.callsite.GetEffectivePojoPropertySite.getProperty(GetEffectivePojoPropertySite.java:61)
	at org.codehaus.groovy.runtime.callsite.AbstractCallSite.callGetProperty(AbstractCallSite.java:227)
	at hudson.model.View.main$_run_closure1.doCall(main.groovy:15)
	at sun.reflect.GeneratedMethodAccessor382.invoke(Unknown Source)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:55)
	at java.lang.reflect.Method.invoke(Method.java:618)
	at org.codehaus.groovy.reflection.CachedMethod.invoke(CachedMethod.java:90)
	at groovy.lang.MetaMethod.doMethodInvoke(MetaMethod.java:233)
	at org.codehaus.groovy.runtime.metaclass.ClosureMetaClass.invokeMethod(ClosureMetaClass.java:272)
	at groovy.lang.MetaClassImpl.invokeMethod(MetaClassImpl.java:903)
	at org.codehaus.groovy.runtime.callsite.PogoMetaClassSite.callCurrent(PogoMetaClassSite.java:66)
	at org.codehaus.groovy.runtime.callsite.AbstractCallSite.callCurrent(AbstractCallSite.java:141)
	at hudson.model.View.main$_run_closure1.doCall(main.groovy)
	at sun.reflect.GeneratedMethodAccessor381.invoke(Unknown Source)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:55)
	at java.lang.reflect.Method.invoke(Method.java:618)
	at org.codehaus.groovy.reflection.CachedMethod.invoke(CachedMethod.java:90)
	at groovy.lang.MetaMethod.doMethodInvoke(MetaMethod.java:233)
	at org.codehaus.groovy.runtime.metaclass.ClosureMetaClass.invokeMethod(ClosureMetaClass.java:272)
	at groovy.lang.MetaClassImpl.invokeMethod(MetaClassImpl.java:903)
	at groovy.lang.Closure.call(Closure.java:415)
	at groovy.lang.Closure.call(Closure.java:409)
	at org.kohsuke.stapler.jelly.groovy.JellyBuilder$1.run(JellyBuilder.java:264)
	at org.kohsuke.stapler.jelly.CallTagLibScript$1.run(CallTagLibScript.java:99)
	at org.apache.commons.jelly.tags.define.InvokeBodyTag.doTag(InvokeBodyTag.java:91)
	at org.apache.commons.jelly.impl.TagScript.run(TagScript.java:269)
	... 111 more
```


## 问题分析
这是jenkins 里使用了 build-pipeline-plugin，这个插件解析有问题。
目前，这个bug已经有人修复，处于resolved状态，等待异常报告者验收，修改代码目前是应该处于未发布状态。

**参考**：[Jenkins gives strack trace on main page](https://issues.jenkins-ci.org/browse/JENKINS-24102)


## 解决方案
1. 重启jenkins服务(自己发现的)
服务出异常后，发现只要jenkins页面刷新后，jenkins cpu使用率会达到200％+，物理内存占用1.1g，虚拟内存占用2.7g。不知道跟这个异常有没关系。

2. 尝试升级build-pipeline-plugin。

3. 尝试升级jenkins版本。