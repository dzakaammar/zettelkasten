---
date: 2021-Aug-19
tags: ["article", "ddd"]
---

# [Domain-Driven Design: Everything You Always Wanted to Know About it, But Were Afraid to Ask](https://medium.com/ssense-tech/domain-driven-design-everything-you-always-wanted-to-know-about-it-but-were-afraid-to-ask-a85e7b74497a)

## Summary
- software entropy, semakin codebase berkembang semakin sulit me-*maintain* code secara ideal jika tanpa rule yang strict dan jelas
- MVC hanya mengatur pembagian layer tapi tidak menjelaskan bagaimana mengatur *responsibility* dan juga *bounderies* 
- Contoh sederhana adalah *naming*. Komunikasi yang baik adalah kunci untuk mengimplementasi sistem yang sesuai dengan kebutuhan bisnis dan *goals* nya serta menghindari misinterpretasi. Kita harus membuat rule yang cukup jelas dalam *naming*, tujuannya memfasilitasi komunikasi dengan bisnis *stakeholder*.
- Domain adalah :

> A specific sphere of activity or knowledge that defines a set of common requirements, terminology, and functionality on which the application logic works to solve a problem.

- DDD adalah pendekatan software design yang melekatkan implementasi sistem dengan model yang akan selalu berubah/berevolusi dan mengenyampingkan hal-hal yang bersifat infrastruktur, programming language dll. Fokus ke bisnis problem.
- Relationship dari setiap bounded context bisa bermacam-macam:
	- [Anti-Corruption layer](202108211902_anti_corruption_layer)
	- [Conformist](202108211906_conformist)
	- [Customer/Supplier](202108221032_customer_supplier)
	- [Shared Kernel](202108221037_shared_kernel)
	- ???

- Developer harus berkolaborasi dengan domain experts menggunakan *ubiquitous language*

## Questions and Notes
- Relasi dari bounded context hanya dijelaskan secara singkat. Harus menemukan definisi yang lebih jelas dan contoh yang lebih konkret
- Artikel ini menjelaskan secara sederhana term-term yang ada di DDD
- Bagaimana hubungan DDD dengan internal type dari programming language, apakah ada kesinambungan?
- Layering dan pattern di DDD apakah harus mengikuti aturan (application, interface, infrastructure, dll) atau bisa mengikuti idiomatic dari programming language yang digunakan?
- 


## Terms
- [[202108192317_software_entropy]]
- [[202108211849_bounded_context]]
- [[202108211914_ubiquitous_language]]
- [[202108221047_entities]]
- [[202108221053_value_objects]]
- 