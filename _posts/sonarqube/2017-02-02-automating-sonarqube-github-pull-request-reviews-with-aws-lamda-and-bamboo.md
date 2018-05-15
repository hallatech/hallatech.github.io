---
layout: "post"
title: "Automating Sonarqube GitHub Pull Request Reviews with AWS Lamda and Bamboo"
categories: sonarqube github lambda bamboo
---

The challenge: Implement the [Sonar Github Plugin][sonar-github-plugin-url] to automate `GitHub pull request reviews`.

Basically on a pull request action an event is generated and can be sent as a payload somewhere. Github has numerous pre-configured event integrations or you can create your own webhook. Once you receive the event somewhere you need to trigger the generation of Sonarqube project properties, send to the server in preview mode and send the results back to Github. Simple in principle, but not so easy in practice!

This was probably one of the smallest new tasks to implement but took the longest and most frustrating to achieve, given all the little hoops to jump through along the way. Some of the issues to contend with:

- Github integrations are configured to only send push events.
- Bamboo where I had to run the Sonar analysis doesn't support pulling specific pull-requests
- Not directly related but Sonar only supports leaf node projects, so as we use typical Oracle Commerce/ATG style projects it doesn't fit that model
- Finding a suitable intermediary to trigger the Bamboo API
- AWS Lambda issues with encrypted environment variables

This will be a long post given all the hurdles involved but here goes ...

(<b>Note</b> This was achieved with certain versions of software at the time that may now have additional features to enable this in a different way)

<h2>Github integrations and events</h2>
First lets see what integrations Github give us. Go to your repository and click on the Settings tab and select Integrations and Services

... to be continued ...

[sonar-github-plugin-url]: https://docs.sonarqube.org/display/PLUG/GitHub+Plugin
