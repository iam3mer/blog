---
title: Test de Mutación
date: 2022-12-01 08:00:00 +/-TTTT
categories: ['Buenas Practicas', 'Testing']
tags: [javascript, mutation, testing, stryker]
---

Al trabajar en desarrollo de software es seguro que en algún momento has trabajado con pruebas, al menos lo habrás escuchado. En general lo que más encontramos son pruebas unitarias, pruebas de integración, pruebas funcionales, pruebas end to end; sin embargo, un tipo de prueba que quizá sea nueva para ti son las pruebas de mutación.

Las pruebas de mutación te permite validar la calidad de tus pruebas automatizadas. Así es, este tipo de prueba pone a prueba tus pruebas automatizadas, parece un trabalenguas no? El tema es más simple de lo que parece. Lo que hacen estas pruebas es cambiar partes del código de forma dinámica, de esta manera se introducen smells en el código original y con esto se consigue verificar si nuestras pruebas automatizadas "detectan" estos cambios y fallan como deberían. Con esto es posible detectar puntos débiles para mejorar las pruebas y cubrir más casos.

Para entenderlo mejor pensemos en una aplicación que tiene una función hasOnlyMultipleFiveNumbers, está recibe un array de números enteros y retorna true cuando todos los números son múltiplos de 5, así:

```javascript
module.exports = function hasOnlyMultipleFiveNumbers(array) {
  return array.every(number => number % 5 == 0)
}
```
{:file="index.js"}
{: .nolineno }

En la función anterior se recorre todo el array comprobando si sus elementos son números múltiplos de cinco.

Ahora bien, la prueba unitaria podría ser:

```javascript
const hasOnlyMultipleFiveNumbers = require('.')

describe('hasOnlyMultipleFiveNumbers', () => {
  it('Debería retornar true cuando el argumento solo contiene elementos múltiplos de cinco.', () => {
    const result = hasOnlyMultipleFiveNumbers([5, 10, 15, 10, 25, 30, 35, 40])
    expect(result).toBe(true)
  })
})
```
{:file="index.spec.js"}
{: .nolineno }

La pregunta que nos debemos hacer es, que tan buena es esta prueba?. Podríamos pensar que esta prueba es suficiente, sin embargo, lo que deberíamos hacer más allá de creer en nuestra intuición o en la experiencia, es pasar una herramienta de mutación, para que esta cambie el código original y verifique que la prueba unitaria detecta ese cambio.

Un ejemplo de lo que haría la prueba de mutación, es cambiar el criterio con el que se esta validando que el numero sea múltiplo de 5, así:

```javascript
- return array.every(number => number % 5 == 0)
+ return array.every(number => true)
```
{: .nolineno }

Una vez realizado el cambio, el test de mutación ejecutara la prueba unitaria. Al fallar las pruebas unitarias estaremos comprobando que estas son lo suficientemente buenas para detectar este cambio, de lo contrario, se debe mejorar la prueba unitaria para evitar que los smells pasen.

Para el ejemplo, la prueba unitaria no fallaría, nuestra prueba entonces no es buena. Esto sucede dado que nuestra prueba unitaria tiene un único camino, el correcto, cuando todos los números son múltiplos de 5. Vamos a agregar una nueva prueba donde se cubra el caso contrario o erróneo.

```javascript
const hasOnlyMultipleFiveNumbers = require('.')

describe('hasOnlyMultipleFiveNumbers', () => {
  it('Debería retornar true cuando el argumento solo contiene elementos múltiplos de cinco.', () => {
    const result = hasOnlyMultipleFiveNumbers([5, 10, 15, 20, 25, 30, 35, 40])
    expect(result).toBe(true)
  })
})

describe('hasOnlyMultipleFiveNumbers', () => {
  it('Debería retornar falso cuando el argumento contiene elementos que no son múltiplos de cinco.', () => {
    const result = hasOnlyMultipleFiveNumbers([5, 10, 15, 23, 25, 33, 35, 40])
    expect(result).toBe(false)
  })
})
```
{:file="index.spec.js"}
{: .nolineno }

Ahora las pruebas unitarias fallan ante la mutación anterior, esto nos indica que las pruebas de mutación han cumplido con su trabajo, y nuestros test automatizados ahora tienen una mejor cobertura y calidad!