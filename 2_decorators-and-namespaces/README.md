**Декораторы**

Предоставляют возможность добавлять метаданные для классов и функций.<br>
По своей сути - это просто функции, принимающие конструктор закрепленного класса.<br>
Декораторы - это экспериментальный инструмент. Для того, чтобы они работали, нужно добавить в tsconfig поле `"experimentalDecorators": true`.

```ts
// декоратор
function logger(constrFn: Function) {
    console.log(constrFn);
}
// функция-обертка для передачи параметров в декораторы
function shouldLog(flag: boolean): any {
    return flag ? logger : null;
}
// закрепить декоратор log за классом
@logger
class User {
    constructor(public name: string, public age: number) {
        console.log('I am new user');
    }
}
// передача параметра в функцию-обертку
@shouldLog(true)
class User_2 {
    constructor(public name: string, public age: number) {
        console.log('I am new user');
    }
}
```
Еще один пример использования декораторов
```ts
// декоратор
function addShowAbility(constructorFn: Function) {
    constructorFn.prototype.showHTML = function() {
        const pre = document.createElement('pre');
        pre.innerHTML = JSON.stringify(this);
        document.body.appendChild(pre);
    }
}
// класс
@addShowAbility
class User {
    constructor(public name: string, public age: number, public job: string) {}
}

const user = new User('ts', 8, 'Frontend');
// принудительно добавить тип any
(<any>user).showHTML();
```

**Namespace**

Для локализации сущностей - практически аналогично тому, чтобы положить все в объект.

```ts
namespace Util {
   export function isEmpty(arg: any): boolean {
      return !arg;
   }

   export function isUndefined(arg: any): boolean {
      return typeof arg === 'undefined';
   }

   export const PI = Math.PI;
   export const EXP = Math.E;
}

console.log(Util.isEmpty('')); // true
console.log(Util.isEmpty('str')); // false
```