**Generics**

Два типа для примера:
```ts
type UserType = {
  firstName: string,
  lastName: string,
  age: number,
}

type PhotoType = {
  large: string,
  small: string,
}
```

Пример типа ответа сервера:
```ts
// Ответ с сервера с одним из типов
type ServerResponseType = {
  errorCode: number,
  messages: Array<string>,
  data: UserType,
}
// Но этот тип работает только для UserType и не работает для PhotoType
const response: ServerReponseType = {
  errorCode: 1,
  messages: ['it', 'kamasutra'],
  data: {
    firstName: 'Dmitry',
    lastName: 'Kuzyuberdin',
    age: 32,
  }
}
```

Вариант типа ответа с использованием generics:
```ts
// Теперь можно использовать как с UserType, так и с PhotoType
type ServerResponseType<D> = {
  errorCode: number,
  messages: Array<string>,
  data: D,
}
// При использовании ServerResponseType необходимо будет уточнить с каким типом идет работа
const response: ServerReponseType<PhotoType> = {
  errorCode: 1,
  messages: ['it', 'kamasutra'],
  data: {
    large: 'path/to-large',
    small: 'path/to-small',
  }
}
// Тоже самое с массивом - массив объектов UserType
const example: Array<UserType> = [ ... ]
```

**typeof**

Можно автоматически выводить типы (infer types):
```ts
const initialState = {
  age: 10,
  name: 'Dimych',
  photo: null,
}

type StateType = typeof initialState;

const reducer = (state: StateType) => {
  return state;
}
```

**as**

Если в примере выше вместо null в поле photo захочется присвоить что-то другое:
```ts
const initialState = {
  age: 10,
  name: 'Dimych',
  user: null as UserType | null,
  photo: null as PhotoType | null,
}

const reducer = (state: StateType) => {
  // PhotoType
  state.photo = {
    large: '',
    small: '',
  }

  return state;
}
```

Для сокращения записей с as выше можно использовать generics:
```ts
type Nullable<MainType> = null | MainType

const initialState = {
  age: 10,
  name: 'Dimych',
  user: null as Nullable<UserType>,
  photo: null as Nullable<PhotoType>,
}

```

**ReturnType, as const**

```ts
// action-creator
const AC1 = (age: number) => ({ type: 'SET_AGE', age });
type AC1Type = ReturnType<typeof AC1>
// type не обязательно ставить 'SET_AGE' - TypeScript ругаться не будет.
const action1: AC1Type = { type: 'WRONG_TYPE', age: 21 }
// с использованием as const
const AC2 = (firstName: string, lastName: string) => ({ type: 'SET_NAME', firstName, lastName } as const);
type AC2Type = ReturnType<typeof AC2>
// теперь в type обязательно должен быть 'SET_NAME', как у AC2
const action2: AC1Type = { type: 'SET_NAME', age: 21 }
```

**Conditional types**

```ts
// Если переданный тип может быть унаследован от 'user', то верни UserType
// А иначе PhotoType
type HipHop<T> = T extends 'user' ? UserType :
  T extends 'photo' ? PhotoType : number

const a: HipHop<'user'> = {
  firstName: 'Dmitry',
  lastName: 'Kuzyuberdin',
  age: 32,
}
// Можно использовать "или", тогда проверка пройдет дважды
const b: HipHop<'user' | 'photo'> = {
  large: 'path-to-large',
  small: 'path-to-small',
}
```

**InferValueTypes**

```ts
import * as actions from 'action-creators';
// Можно было создать общий тип для всех actions
type ActionsType = ReturnType<typeof AC1> | ReturnType<typeof AC2>
// И даже еще больше сократить с помощью InferValueTypes
type InferValueTypes<T> = T extends { [key: string ]: infer U }
  ? U
  : never;

type ActionTypes = ReturnType<InferValueTypes<typeof actions>>;
```

