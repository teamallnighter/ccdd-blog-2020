---
title: "Honeypot Spam Filter"
categories:
  - Form Processing 
comments: true
tags:
  - Netlify
  - Forms
last_modified_at: 2020-09-03T08:25:52-05:00
excerpt: "A honeypot uses a hidden field to trick bots..."
author: "Chris Connelly"
image:
  path: /assets/images/carbon.png
---

This is a spam filtering technique where we put a hidden field 
into the form. A human can't see it and it's not required. 

```html
<!--We use the following to tell netlify what we are doing and what field is the honeypot-->
     <form class="form-template-v3" method="POST" netlify-honeypot="bot-field" data-netlify="true">
```

```html
<!--This is the field. -->
          <p class="hidden" style="display:none">
            <label>Don’t fill this out if you're human: <input name="bot-field" /></label>
          </p>
```


![The honeypot form result](/assets/images/carbon.png)

However a bot WILL fill it in. Then we know it's spam. 

![The honeypot form result](/assets/images/honeypotform.png)

It's a lot eaiser on the user than using a captcha. 



