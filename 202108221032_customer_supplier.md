---
date: 2021-Aug-22
tags: ["glossary", "ddd", "ddd_relationship"]
---

# Customer/Supplier
Upstream bounded context menyuplai service kepada downstream bounded context, dan downstream bounded context bertindak sebagai customer. Downstream bounded context menentukan kebutuhan dan meminta perubahan untuk memenuhi kebutuhan mereka kepada upstream bounded context

- Upstream service/bounded context needs to communicate if they want to make changes to the downstream service because every change in the upstream service will impact the downstream
