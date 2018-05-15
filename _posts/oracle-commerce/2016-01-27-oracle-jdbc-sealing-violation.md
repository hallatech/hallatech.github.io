---
layout: "post"
title: "Oracle JDBC Sealing Violation"
categories: oracle-xe JDBC
---

<b>Oracle Commerce 11.2</b> now supports <b>JDK 1.8</b>. I ran into the following error when running CIM with WebLogic 12.1.3 and JDK8:

{% highlight bash %}
**** info    Wed Mar 04 13:07:17 CET 2015 1425470837190 atg.cim.task.ant.utility.AntLogger 
[exec] Caused by: java.lang.SecurityException: sealing violation: package oracle.jdbc.internal is sealed
{% endhighlight %}

This error is normally attributed to multiple versions of the same class on the CLASSPATH, however in this case without a pre classpath being set, CIM derives all the CLASSPATH elements itself, and I couldn't find any duplications.

As we use either JBoss or WebLogic we pull in the JDBC driver from a Maven repository via a Gradle configuration, so I normally supply this version to the JDBC library path for CIM. You can of course also use the JDBC driver in the Oracle XE installation:

`/u01/app/oracle/product/11.2.0/xe/jdbc/lib/ojdbc6.jar`

or from the WebLogic installation itself:

`<path>/Oracle/Middleware/oracle_common/modules/oracle.jdbc_12.1.0/ojdbc*.jar`

The mistake I made of course was to use the <b>ojdbc6.jar</b> with <b>JDK8</b> as I have been doing for <b>JDK7</b> and the supported driver for Oracle XE.

Using the <b>ojdbc7.jar</b> with <b>JDK8</b> resolves the sealing violation issue.
