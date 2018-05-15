---
layout: "post"
title: "Sonarqube new install fails with \"Current version is too old\""
categories: sonarqube
---

Installing a fresh version of the latest `Sonarqube 6.0` on an `EC2` instance running `MySql 5.7` resulted in the following error messages on startup with an immediate shutdown:

{% highlight bash %}
2016.08.17 02:05:55 ERROR web[o.a.c.c.C.[.[.[/]] Exception sending context initialized event to listener instance of class org.sonar.server.platform.PlatformServletContextListener
org.sonar.api.utils.MessageException: Current version is too old. Please upgrade to Long Term Support version firstly.
2016.08.17 02:05:55 ERROR web[o.a.c.c.StandardContext] One or more listeners failed to start. Full details will be found in the appropriate container log file
2016.08.17 02:05:55 ERROR web[o.a.c.c.StandardContext] Context [/] startup failed due to previous errors
{% endhighlight %}

This may be misleading because its a fresh install rather than an upgrade, but the database even if empty must undergo a migration from scratch to the latest version. In my case I hadn't given the user enough privileges and had to startup 3 times until I got the privileges correct. This was the underlying cause of the error above as by the time it tried to start properly the DB was already partially populated and so it thought a migration from a previous version was required.

To confirm this, run the following query against the database:

<b>SELECT * FROM schema_migrations;</b>

The query should return nothing. If anything you need to start again.

The easiest solution for a new install is to drop and recreate the database and start Sonar again.

Solution thanks to [Julien Lancelot from SonarSource][sonar-url]:

[sonar-url]: https://groups.google.com/forum/#!topic/sonarqube/4nxZhBcSdTI