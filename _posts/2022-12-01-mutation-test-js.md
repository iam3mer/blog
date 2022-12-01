---
title: Test de Mutación en JavaScript
date: 2022-12-01 08:00:00 +/-TTTT
categories: ['Buenas Practicas', 'Testing']
tags: [javascript, mutation, testing, stryker]
---

Al trabajar en desarrollo de software es seguro que en algun momento has trabajado con pruebas, al menos lo habras escuchado. En general lo que más encontramos son las pruebas unitarias, pruebas de integración, pruebas funcionales, pruebas end to end; sin embargo, un tipo de prueba que quiza sea nuevo para ti son las pruebas de mutación.

Las pruebas de mutación te permite validar la calidad de tus pruebas automatizadas. Así es, este tipo de prueba pone a prueba tus pruebas automatizadas, parece un trabalenguas no? El tema es más simple de lo que parece. Lo que hacen estas pruebas es cambiar partes del codigo de forma dinamica, de esta manera se introducen smells en el codigo original y con esto se consigue verificar si nuestras pruebas automatizadas "detectan" estos cambios y fallan como deberían. Con esto es posible detectar puntos debiles para mejorar las pruebas y cubrir los casos.

Para entenderlo mejor pensemos en una aplicación que tiene una función hasOnlyMultipleFiveNumbers recibe un array de numeros enteros y retorna true cuando todos los numeros son multiplos de 5, así:

```javascript
module.exports = function hasOnlyMultipleFiveNumbers(array) {
  return array.every(number => number % 5 == 0)
}
```
{:file="index.js"}

En la función anterior se recorre todo el array comprobando si sus elementos son números multiplos de cinco.

Ahora bien, la prueba unitaria podría ser:

```javascript
const hasOnlyMultipleFiveNumbers = require('.')

describe('hasOnlyMultipleFiveNumbers', () => {
  it('Debería retornar true cuando el argumento solo contiene elementos multiplos de cinco.', () => {
    const result = hasOnlyMultipleFiveNumbers([5, 10, 15, 10, 25, 30, 35, 40])
    expect(result).toBe(true)
  })
})
```
{:file="index.spec.js"}

La pregunta que nos debemos hacer es que tan buena es esta prueba. Podríamos pensar que esta prueba es suficiente, sin embargo, lo que deberiamos hacer más alla de creer en nuestra intuición o en la experiencia, es pasar una herramienta de mutación, para que esta cambie el codigo de la función y verifique que la prueba unitaria detecte ese cambio.

Un ejemplo de lo que haría la prueba de mutación, es cambiar el criterio con el que se esta validando que el numero sea multiplo de 5, así:

```javascript
return array.every(number => number % 5 == 0)
return array.every(number => true)
```

Una vez realizado el cambio, el test de mutación ejecutara la prueba unitaria. Al fallar las pruebas unitarias estaremos comprobando que estas son lo suficiente mente buenas para detectar este cambio, de lo contrario, se debe mejorar la prueba unitaria para evitar que los smells pasen.

Para el ejemplo la prueba unitaria no fallaría, nuestra prueba entonces es buena. Esto es facil de ver puesto que nuestra prueba unitaria tiene un unico camino, cuando todos los numeros son multiplos de 5.