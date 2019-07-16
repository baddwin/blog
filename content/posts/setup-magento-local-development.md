---
title: "Setup Magento Local Development with nginx"
date: 2019-07-16T21:15:52+07:00
draft: false
description: "Setup Magento Local Development on nginx, Fedora 30"
tags: ["magento", "php", "development", "nginx", "fedora"]
---

Menyesuaikan _permission_ file dan folder tertentu.

```
find var generated vendor pub/static pub/media app/etc -type f -exec chmod g+w {} +
find var generated vendor pub/static pub/media app/etc -type d -exec chmod g+ws {} +
chown -R :www-data .
```

Setelah setup server lokal selesai, dilanjutkan dengan
instalasi Magento.

![magento check requirements to install](/img/2019/07/magento-install-1.png)

![magento database config](/img/2019/07/magento-install-2.png)

![magento web config](/img/2019/07/magento-install-3.png)

![magento customization](/img/2019/07/magento-install-4.png)

![magento admin account](/img/2019/07/magento-install-5.png)

![magento installation progress](/img/2019/07/magento-install-6.png)

![magento success install](/img/2019/07/magento-install-7.png)

![magento login admin](/img/2019/07/magento-install-8.png)