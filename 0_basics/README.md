**TypeScript**

Язык программирования, написанный поверх JavaScript. В конечном итоге в браузере он компилируется в JavaScript.

**Базовые команды**

`npm install -g typescript` - установка typescript.<br>
`tsc --init` - создать базовый файл конфигурации typescript (tsconfig.json).<br>
`tsc filename` - скомпилировать typescript в javascript.

**Базовые типы**

1. let str: `string`.
2. let bool: `boolean`.
3. let num: `number`.
4. let x: any - любое значение.
5. Можно также перечислить несколько типов: `string | number`.

**Массивы**

```ts
// tuple - неизменяемый массив.
let array: [string, number] = ['str_1', 2];
// массив чисел.
let numArray: number[] = [1, 2, 3];
// массив строк - другой способ задания массива. Array - тип, <number> - из чего состоит.
let anotherNumArray: Array<number> = [1, 2, 3];
```

**Функции**

```ts
const myName: string = 'typescript';
const myAge: number = 8;
// тип возвращаемого значения.
function getMyName(): string {
    return myName;
}

const getMyAge = (): number => myAge;
// валидация параметров
const returnNumber = (num: number) => num;
// с параметрами по умолчанию
const logNumberWithInitialValue = (num: number = 0) => num;
// ничего не возвращающая функция (void)
const logSomething = (str: string): void => {
    console.log(str);
}
```

**Объекты**

```ts
const obj: Object = { prop: 1 }; // OK
const nullObj: Object = null; // OK
// типизированный объект
const user: { name: string, age: number, logName: () => void, parents: string[] } = {
    name: 'typescript',
    age: 8,
    parents: ['javascript', 'microsoft'],
    logName(): void {
        console.log(this.name);
    },
    getJobs
};
// тип пользователя с необязательным полем getName
type UserType = { name: string, age: number, getParents: () => string[], parents: string[], getName?: () => string }

const user_2: UserType = {
    name: 'ecmascript_6',
    age: 5,
    parents: ['Ecma International'],
    getParents(): string[] {
        return this.parents;
    }
};
```

**Специальные типы**

Enum - различные константы для дальнейшего использования в качестве типов.

```ts
enum Job {
    Frontend,
    Backend,
    Designer
}

const job: Job = Job.Backend; // job === 1

enum JobWithValue {
    Frontend,
    Backend = 50,
    Designer
}

const job_2: JobWithValue = JobWithValue.Frontend; // job === 0
const job_3: JobWithValue = JobWithValue.Backend; // job === 50
const job_4: JobWithValue = JobWithValue.Designer; // job === 51
```

Never - представляет собой тип, значения которого никогда не встречаются.

```ts
// нельзя указать тип возвращаемого значения - void
function throwNewError(err: string): never {
    throw new Error(err);
}
// функция, возвращающая never не имеет конечной точки
function infiniteLoop(): never {
    while (true) {
        // something...
    }
}
```

`null` и `undefined`.<br>
По умолчанию эти типы являются подтипами всех остальных типов.<br>
Это означает, что их можно присваивать, например, такому типу как `number`.<br>
Хотя если использовать флаг для компилятора `--strictNullChecks`, `null` и `undefined` можно присваивать только `any` (за исключением того, что `undefined` все равно можно присвоить типу `void`).<br>

```ts
let u: undefined = undefined;
let n: null = null;
// эти типы помогают избегать многих ошибок. Можно использовать объединение:
let str: string | null | undefined = "str";
```

**Классы**

```ts
class User {
    public name: string; // по умолчанию также public
    private isTeacher: boolean; // недоступна снаружи класса
    protected age: number = 30; // можно обращаться в наследуемых классах

    constructor(name: string) {
        this.name = name;
    }
    // можно поставить и методы private/protected
    public getAge(): number {
        return this.age;
    }
}
// User { name: 'username', age: 30, __proto__: Object }
const user = new User('username');
```

```ts
// абстрактные классы, нужны для того, чтобы от них наследовались. С их помощью нельзя создавать объекты
abstract class Car {
    model: string;
    year: number = 2010;

    getCarYear() {
        return this.year; 
    }
    // метод, который необходимо реализовать в классе-наследнике
    abstract logInfo(info: string): void;
}
```

**Интерфейсы**

```ts
interface ILength {
    length: number;
}

function getLength(variable: { length: number }): void {
    console.log(variable.length);
}

function getLengthWithInterface(variable: ILength): void {
    console.log(variable.length);
}
// интерфейсы также могут принимать необязательные параметры
interface IUser {
    name: string;
    age?: number;
    logInfo(info: string): void;
}

const user: IUser = {
    name: 'username',
    logInfo(info: string) {
        console.log('Info', info);
    }
}
// можно также создавать классы по интерфейсам
class User implements IUser {
    name: string = 'username';
    age: number = 20;

    logInfo(info: string) {
        console.log(info);
    }
}
```