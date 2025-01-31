# some

::: info
이 함수는 호환성을 위한 `es-toolkit/compat` 에서만 가져올 수 있어요. 대체할 수 있는 네이티브 JavaScript API가 있거나, 아직 충분히 최적화되지 않았기 때문이에요.

`es-toolkit/compat`에서 이 함수를 가져오면, [lodash와 완전히 똑같이 동작](../../../compatibility.md)해요.
:::

배열이나 객체의 요소 중 주어진 조건을 만족하는 요소가 있는지 확인해요.

조건은 여러 방법들로 명시할 수 있어요.

- **검사 함수**: 각각의 요소에 대해서 검사하는 함수를 실행해요. 처음으로 `true`를 반환하게 하는 값이 선택돼요.
- **부분 객체**: 주어진 객체와 부분적으로 일치하는 첫 번째 요소가 선택돼요.
- **프로퍼티-값 쌍**: 해당 프로퍼티에 대해서 값이 일치하는 첫 번째 요소가 선택돼요.
- **프로퍼티 이름**: 해당 프로퍼티에 대해서 참으로 평가되는 값을 가지는 첫 번째 요소가 선택돼요.

조건이 제공되지 않으면, 배열이나 객체에 참으로 평가되는 요소가 있는지 확인해요.

## 인터페이스

```typescript
function some<T>(arr: T[]): boolean;
function some<T>(arr: T[], predicate: (item: T, index: number, arr: any) => unknown): boolean;
function some<T>(arr: T[], predicate: [keyof T, unknown]): boolean;
function some<T>(arr: T[], predicate: PropertyKey): boolean;
function some<T>(arr: T[], predicate: Partial<T>): boolean;

function some<T extends Record<string, unknown>>(object: T): boolean;
function some<T extends Record<string, unknown>>(
  object: T,
  predicate: (value: T[keyof T], key: keyof T, object: T) => unknown
): boolean;
function some<T extends Record<string, unknown>>(object: T, predicate: Partial<T[keyof T]>): boolean;
function some<T extends Record<string, unknown>>(object: T, predicate: [keyof T[keyof T], unknown]): boolean;
function some<T extends Record<string, unknown>>(object: T, predicate: PropertyKey): boolean;
```

### 파라미터

- `arr` (`T[]`) 또는 `object` (`T`): 반복할 배열.

::: info `arr`는 `ArrayLike<T>`일 수도 있고, `null` 또는 `undefined`일 수도 있어요

lodash와 완벽하게 호환되도록 `every` 함수는 `arr`을 다음과 같이 처리해요:

- `arr`가 `ArrayLike<T>`인 경우 `Array.from(...)`을 사용하여 배열로 변환해요.
- `arr`가 `null` 또는 `undefined`인 경우 빈 배열로 간주돼요.

:::

::: info `object`는 `null` 또는 `undefined`일 수도 있어요

lodash와 완벽하게 호환되도록 `every` 함수는 `object`를 다음과 같이 처리해요:

- `object`가 `null` 또는 `undefined`인 경우 빈 객체로 변환돼요.

:::

- `predicate`:

  - 배열의 경우:

    - **검사 함수** (`(item: T, index: number, arr: any) => unknown`): 요소, 인덱스, 배열을 받아 조건을 만족하면 `true`를 반환하는 함수.
    - **부분 객체** (`Partial<T>`): 일치시킬 프로퍼티와 값들을 명시한 부분 객체.
    - **프로퍼티-값 쌍** (`[keyof T, unknown]`): 첫 번째가 일치시킬 프로퍼티, 두 번째가 일치시킬 값을 나타내는 튜플.
    - **프로퍼티 이름** (`PropertyKey`): 참으로 평가되는 값을 가지고 있는지 확인할 프로퍼티 이름.

  - 객체의 경우:

    - **검사 함수** (`(value: T[keyof T], key: keyof T, object: T) => unknown`): 값, 키, 객체를 받아 조건을 만족하면 `true`를 반환하는 함수.
    - **부분 값** (`Partial<T[keyof T]>`): 값과 일치시킬 부분 값을 명시한 부분 객체.
    - **프로퍼티-값 쌍** (`[keyof T[keyof T], unknown]`): 첫 번째가 일치시킬 프로퍼티, 두 번째가 일치시킬 값을 나타내는 튜플.
    - **프로퍼티 이름** (`PropertyKey`): 참으로 평가되는 값을 가지고 있는지 확인할 프로퍼티 이름.

### 반환 값

(`boolean`): 조건을 만족하는 요소가 있는지 여부.

## 예시

### 배열의 경우

```typescript
import { every } from 'es-toolkit/compat';

// 검사 함수를 쓰는 경우
const items = [1, 2, 3, 4, 5];
const result = every(items, item => item > 0);
console.log(result); // true

// 부분 객체를 쓰는 경우
const items = [
  { id: 1, name: 'Alice' },
  { id: 2, name: 'Bob' },
];
const result = every(items, { name: 'Bob' });
console.log(result); // false

// 프로퍼티-값 쌍을 쓰는 경우
const items = [
  { id: 1, name: 'Alice' },
  { id: 2, name: 'Bob' },
];
const result = every(items, ['name', 'Alice']);
console.log(result); // false

// 프로퍼티 이름을 쓰는 경우
const items = [
  { id: 1, name: 'Alice' },
  { id: 2, name: 'Bob' },
];
const result = every(items, 'name');
console.log(result); // true
```

### 객체의 경우

```typescript
import { every } from 'es-toolkit/compat';

// 검사 함수를 쓰는 경우
const obj = { a: 1, b: 2, c: 3 };
const result = every(obj, value => value > 0);
console.log(result); // true

// 부분 객체를 쓰는 경우
const obj = { a: { id: 1, name: 'Alice' }, b: { id: 2, name: 'Bob' } };
const result = every(obj, { name: 'Bob' });
console.log(result); // false

// 프로퍼티-값 쌍을 쓰는 경우
const obj = { alice: { id: 1, name: 'Alice' }, bob: { id: 2, name: 'Bob' } };
const result = every(obj, ['name', 'Alice']);
console.log(result); // false

// 프로퍼티 이름을 쓰는 경우
const obj = { a: { id: 1, name: 'Alice' }, b: { id: 2, name: 'Bob' } };
const result = every(obj, 'name');
console.log(result); // true
```
