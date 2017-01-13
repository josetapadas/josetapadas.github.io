---
layout: none
title: Sample facebook redirect
permalink: /redirecttrick
---

{% raw %}
<html>
  <head>
    <title>fb-redirect-poc</title>
    <meta property="og:title" content="LOLA"/>
    <meta property="og:image" content="https://cloud.githubusercontent.com/assets/882606/21937253/94ea7f10-d984-11e6-93f4-5f7d829027f1.jpg"/>
    <meta property="og:description" content="LOLA and Novis partnered to create a fun and stylish pouch to tote your tampons">
  </head>
  <body>
    Shazaam!
    <script>
      location.href = 'https://lola-x-novis.myshopify.com/';
    </script>
  </body>
</html>
{% endraw %}

