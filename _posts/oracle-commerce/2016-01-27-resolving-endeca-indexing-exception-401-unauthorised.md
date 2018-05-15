---
layout: "post"
title: "Resolving EndecaIndexing Exception 401 Unauthorised"
categories: oracle-commerce endeca
---

Following on from having to resolve the <b>401</b> error on CRS initialisation I then got a similar error when running the baseline index after a full deployment from the BCC or running manually from

`dyn/admin/nucleus/atg/commerce/endeca/index/ProductCatalogSimpleIndexingAdmin`

{% highlight bash %}
**** Error    Wed Jan 27 02:50:34 +00:00 2016    1453863034854    /atg/commerce/endeca/index/ProductCatalogSimpleIndexingAdmin    ---    atg.repository.search.indexing.IndexingException: atg.repository.search.indexing.IndexingException: Exception while submitting configuration for application CRS
**** Error    Wed Jan 27 02:50:34 +00:00 2016    1453863034854    /atg/commerce/endeca/index/ProductCatalogSimpleIndexingAdmin    Caused by (#2):com.endeca.repository.importer.ImporterException: Unable to connect to config repository on localhost.localdomain:8006 with user admin, Response:HTTP/1.1 401 Unauthorized
{% endhighlight %}

In trying to find out how to reset the password for this scenario go to the main Endeca configuration component:

`dyn/admin/nucleus/atg/endeca/ApplicationConfiguration/`

Click through to the <b>CredentialStoreManager</b> component:

`dyn/admin/nucleus/atg/dynamo/security/opss/csf/CredentialStoreManager`

![CredentialStoreManager]({{ "/assets/csm1.png" | absolute_url }})

It has a few actions to manage the credentials.

Select Action: <i>Delete Credential</i> and click the <i>Select</i> button

Select <i>ifcr</i> and click the <i>Delete Credential</i> button

Select Action: <i>Create Login Credential</i> and click the <i>Select</i> button

Enter:

MapName=<b>endecaToolsAndFrameworks</b>  
Credential Key Name=<b>ifcr</b>  
Username=<b>admin</b>  
Password and Confirm Password = `<the Workbench login password>`  

Click <i>Submit Credentials</i> button

Your password has been reset and you can return to

`dyn/admin/nucleus/atg/commerce/endeca/index/ProductCatalogSimpleIndexingAdmin`

and do a new baseline update which should now succeed.