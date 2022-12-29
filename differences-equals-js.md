# Difference between == and ===

### 🇬🇧

The == and === operands are used to compare if two values are equal.

The == operand loosely compares two values, thus it can be used in cases where the data type of the operand isn't important. For e.g., imagine a form entry where you've asked students their roll no. It is possible that some will enter it in string form and some in number form. In such cases, we can use the == operator to verify the data without the database.

The === operand strictly compares two values, thus it is used in the places where the data type of the operand is important. Imagine an online coding contest, where an answer is a number in string format. In that case, we'll use the === operator to compare and validate answers.

### Comparison and differences

| ==                                                                                        | ===                                                                                                  |
| ----------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| <sup>Compares two operands                                                                | <sup>Compares two operands                                                                           |
| <sup>returns _true_ if operands have the same value, returns _false_ if the values differ | <sup>returns _true_ only if operands are of same data type and same value, otherwise returns _false_ |
| <sup>Also known as _loose equality_                                                       | <sup>Also known as _strict equality_                                                                 |
| <sup>Follows abstract equality comparison algorithm                                       | <sup>Follows strict equality comparison algorithm                                                    |

### Examples

Following are some examples of the behavior of the == operator:

```javascript
console.log(2 == 2);
//output: true, because 2 and 2 are the same.

console.log(2 == '2');
//output: true, because the string '2' gets converted into a number before comparison.

console.log(false == false);
//output: true, as the operands are the same.

console.log( false == 0 );
//output: true, as the == operator does the type conversion, and then the '0' entity is treated as false.

let student1 = {
name: 'John',
class: 10
}

let student2 = {
name: 'John',
class: 10
}

let student3 = {
name: 'Peter',
class: 10
}

console.log(student1 == student1);
//output: true, as both the operands refer to the same object.

console.log(student1 == student2);
//output: false, as student1 and student2 refer to different objects.

console.log(student1 == student3);
//output: false, as student1 and student3 refer to different objects.

Following are some examples of the behavior of the === operator:
javascript
console.log(2 === 2);
//output: true, because 2 and 2 have the same data type and value

console.log(2 === '2');
//output: false, because the === operator doesn't do the type conversion and the data types of both operands are different.

console.log(false === false);
//output: true, as the operands have the same data type and value.

console.log( false === 0 );
//output: false, as the === operator doesn't do the type conversion and the data types of both operands are different.

let student1 = {
name: 'John',
class: 10
}

let student2 = {
name: 'John',
class: 10
}

let student3 = {
name: 'Peter',
class: 10
}

console.log(student1 === student1);
//output: true, as both the operands refer to the same object.

console.log(student1 === student2);
//output: false, as student1 and student2 refer to different objects.

console.log(student1 === student3);
//output: false, as student1 and student3 refer to different objects.
```

```javascript
null === null; //true
undefined === undefined; //true
null === undefined; //false
null == null; //true
undefined == undefined; //true
null == undefined; //true
```

### 🇮🇹

Gli operatori == e === vengono utilizzati per confrontare se due valori sono uguali.

L'operatore == confronta in modo non vincolante i due valori, quindi può essere usato nei casi in cui il tipo del dato non è importante. Ad esempio, immaginiamo di inserire un modulo in cui si chiede agli studenti il loro numero di matricola. Alcuni possono inserirlo come numero, altri come stringa, in questo caso == non rivela differenze.

L'operatore === compara in modo vincolante i due valori, quindi viene utilizzati se il tipo di dato è significante. Nell'esempio di prima i due valori sarebbero differenti.

### Confronto e differenze

| ==                                                                                                   | ===                                                                                                                  |
| ---------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| <sup>Confronta due operandi                                                                          | <sup>Confronta due operando                                                                                          |
| <sup>ritorna _true_ se gli operandi hanno lo stesso valore, ritorna _false_ se i valori differiscono | <sup>ritorna _true_ solo se gli operandi sono dello stesso tipo e hanno lo stesso valore, altrimenti ritorna _false_ |
| <sup>Conosciuto come _loose equality_                                                                | <sup>Conosciuto come _strict equality_                                                                               |
| <sup>Segue l'algoritmo abstract equality comparison                                                  | <sup>Segue l'algoritmo strict equality comparison                                                                    |
