---
title: Ejemplo LSP Vida Real
date: 2023-01-20 08:00:00 +/-TTTT
categories: ['Buenas Practicas', 'Software', 'Solid']
tags: [development, solid, lsp]
---

Un ejemplo de Liskov Substitution Principle (LSP) en la vida real podría ser el concepto de sustitución de monedas.

Supongamos que tenemos una caja registradora que acepta monedas para realizar pagos. La caja registradora está diseñada para aceptar cualquier tipo de moneda, siempre y cuando cumpla con ciertos requisitos básicos, como tener un valor nominal y un diámetro específico.

Si creamos una nueva moneda, por ejemplo una moneda de adamantio, y queremos usarla en la caja registradora, la caja registradora debería aceptarla sin ningún problema, siempre y cuando cumpla con los requisitos básicos mencionados anteriormente. Esta nueva moneda es una clase derivada de la clase "moneda" y debe ser sustituible por cualquier otra moneda que cumpla con los requisitos básicos.

Este es un ejemplo simple que ilustra cómo el principio LSP se aplica en la vida real. En resumen, se trata de asegurarse de que un objeto derivado pueda ser usado en cualquier lugar donde se espera el objeto base, sin interrumpir el correcto funcionamiento del sistema.