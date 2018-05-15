---
layout: "post"
title: "Oracle Commerce imported projects not appearing in BCC"
categories: oracle-commerce ATG BCC
---

When importing data as projects into the BCC via one of the Oracle Commerce (ATG) utilities such as `startSQLRepository`, the projects will show up on the main BCC Projects panel.

If you or someone has logged into the BCC after a server start but before the data import, generally the projects won't appear. This is because the projects themselves are cached as part of the repository architecture in the same way as user/site data.

To make the new imported projects visible invalidate the caches via the `dyn/admin` console for the BCC instance for the `/atg/epub/PublishingRepository`. Invoke the `invalidateCaches` method in the `Methods` section near the bottom of the page. It is not necessary to invalidate external caches.

Then refresh the BCC home page and the projects should all appear.
