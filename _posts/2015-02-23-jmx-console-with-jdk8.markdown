---
layout: post
title:  "JMX Console doesn't come up with JBoss 6 and JDK 8? We have a jar for you!"
date:   2015-02-23 21:19:00
comments : true
categories: Blog
---
Did your trusty old JMX console on JBoss 6 stopped working once you upgraded java to JDK8? Here is the jar you are looking for.

I recently had to upgrade Java to JDK 8 at work. (I know, it is about time we did that). We are still on JBoss 6 and we use the jmx-console extensively for almost every application we have running on JBoss 6.

Once I upgraded to JDK8, the jmx-console that I usually access at `http://localhost:8080/jmx-console` was broken and was throwing the following stacktrace:

{% highlight bash %}
ERROR [org.apache.jasper.compiler.Compiler] (http-0.0.0.0-8280-2) Compilation error: org.eclipse.jdt.internal.compiler.classfmt.ClassFormatException
	at org.eclipse.jdt.internal.compiler.classfmt.ClassFileReader.<init>(ClassFileReader.java:372) [:6.1.0.Final]
	at org.apache.jasper.compiler.JDTCompiler$1.findType(JDTCompiler.java:213) [:6.1.0.Final]
	at org.apache.jasper.compiler.JDTCompiler$1.findType(JDTCompiler.java:170) [:6.1.0.Final]
	.....
{% endhighlight %}

Some googling led me to [this](https://bz.apache.org/bugzilla/show_bug.cgi?id=56613) bug and it was suggested there that I would need a newer Eclipse ECJ compiler. Why JBoss (or Tomcat internally) is using the Eclipse JDT's Java compiler to compile JSP is beyond me.

Fortunately, the ECJ compiler jar was available in the Maven Repo.

{% highlight xml %}
<dependency>
	<groupId>org.eclipse.jdt.core.compiler</groupId>
	<artifactId>ecj</artifactId>
	<version>4.4.1</version>
</dependency>
{% endhighlight %}

tl;dr
I grabbed the jar from the above (`ecj-4.4.1.jar`) and copied it to my jboss installation's `jboss-6.1.0.Final/lib/endorsed` folder and restarted it. 

That is all I had to do. It has been working great so far and my jmx-console is back as you can see here:
![JMX Console with JDK8](/assets/jboss6_jmx_with_jdk8.jpg)

