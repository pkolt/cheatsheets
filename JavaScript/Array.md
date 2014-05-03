Массивы
=======

Массив это упорядоченная коллекция элементов. Позиция элемента в массиве определяется его индексом.
Первый элемент массива имеет индекс равный нулю. Элементы массива могут иметь любой тип допустимый в JavaScript.

* [Array.prototype](#arrayprototype)
* [Array.prototype.length](#arrayprototypelength)
* [Array.prototype.constructor](#arrayprototypeconstructor)
* [Array.isArray()](#arrayisarray) [es5] [nochg]


Синтаксис
---------

### Создание массива

```javascript
// пустой массив
arr = new Array();

// массив из 10 элементов undefined
arr = new Array(10);

// массив из 3-х элементов
arr = new Array('a', 'b', 'c');
arr = ['a', 'b', 'c'];
```

### Выборка элементов

```javascript
var arr = ['a', 'b', 'c'];

arr[0]; // первый элемент массива
arr[arr.length-1]; // последний элемент массива
```

### Разряженный массив

Разряженный массив это массив количество элементов которого всегда меньше его длины.

```javascript
var arr = ['a', 'b', 'c'];

delete arr[0];

arr.length; // первый элемент удален, но длина массива по-прежнему равна 3
```


Свойства
--------

### Array.prototype

Прототип.

### Array.prototype.length

Длина массива.

### Array.prototype.constructor

Ссылка на функцию-конструктор Array.

Методы
------

### Array.isArray()
#### `Array.isArray(value) -> Boolean`

Возврашает `true` если переданное значение является массивом, иначе `false`.

### Array.from() [es6]

### Array.of() [es6]

### Array.prototype.find() [es6]

### Array.prototype.findIndex() [es6]

### Array.prototype.fill() [es6]

### Array.prototype.unshift() [chg]

### Array.prototype.shift() [chg]
#### Array.prototype.shift() -> Object|undefined

Удаляет первый элемент массива и возвращает его.
После удаления первого элемента, следующие элементы сдвигаются в начало массива на одну позицию.
Если в массиве нет элементов вернет `undefined`.

### Array.prototype.push() [chg]
#### Array.prototype.push(elem, ...) -> Number

Добавляет в конец массива один или более элементов и возвращает новую длину массива.

```javascript
var arr = [];
arr.push('foo', 'bar'); // 2
```

### Array.prototype.pop() [chg]
#### Array.prototype.pop() -> Object|undefined

Удаляет последний элемент массива и возвращает его. При удалении последнего элемента длина массива уменьшается.
Если в массиве нет элементов вернет `undefined`.

### Array.prototype.reverse() [chg]
#### Array.prototype.reverse()

Изменяет порядок следования элементов в массиве на обратный.

### Array.prototype.sort() [chg]

### Array.prototype.splice() [chg]

### Array.prototype.join() [nochg]
#### Array.prototype.join([sep]) -> String

Преобразует каждый элемент массива в строку, выполняет их конкатенацию используя в качестве разделителя запятую.

Если указан необязательный параметр `sep`, то в качестве разделителя будет использоваться он.

### Array.prototype.concat() [nochg]
#### Array.prototype.concat(elem, ...) -> Array

Создает новый массив из исходного добавляя в него переданные элементы.

### Array.prototype.slice() [nochg]

### Array.prototype.forEach() [nochg] [es5]
#### Array.prototype.forEach(func, [ctx])

Вызывает функцию `func` для каждого элемента массива.

Необязательный аргумент `ctx` является контекстом выполнения функции и доступен в `this`.

```javascript
['foo', 'bar', 'spam'].forEach(function(elem, index, arr){
    // ...
});
```

В разряженных массивах функция `func` не вызывается для отсутствующих элементов.

### Array.prototype.map() [nochg] [es5]
#### Array.prototype.map(func, [ctx]) -> Array

Выполняет обход массива передавая каждый элемент функции `func`.
Значения возвращаемые функцией `func` образуют элементы нового массива.

Необязательный аргумент `ctx` является контекстом выполнения функции и доступен в `this`.

```javascript
['foo', 'bar', 'spam'].map(function(elem, index, arr){
    // ...
});
```

### Array.prototype.filter() [nochg] [es5]
#### Array.prototype.filter(func, [ctx]) -> Array

Возвращает новый массив из элементов которые функция `func` оценила как `true`, остальные элементы игнорируются.

Необязательный аргумент `ctx` является контекстом выполнения функции и доступен в `this`.

```javascript
['foo', 'bar', 'spam'].filter(function(elem, index, arr){
    // ...
});
```

### Array.prototype.every() [nochg] [es5]
#### Array.prototype.every(func, [ctx]) -> Boolean

Вернет `true` если все элементы переданные в функцию `func` оцениваются как `true`, иначе вернет `false`.

Необязательный аргумент `ctx` является контекстом выполнения функции и доступен в `this`.

```javascript
['foo', 'bar', 'spam'].every(function(elem, index, arr){
    // ...
});
```

При применении к пустому массиву возвращает `true`.

### Array.prototype.some() [es5]

### Array.prototype.reduce() [nochg] [es5]
#### Array.prototype.reduce(func, [initial]) -> Object

Вычисляет значение из элементов массива. Поочередно вызывает функцию `func` для двух его элементов,
полученный результат вновь используется для вычисления значения со следующим элементом массива.

В аргументе `initial` передается значение добавляемое в начало массива без модификации исходного массива.

Если массив пуст и агумент `initial` не задан, то будет брошено исключение `TypeError`.

```javascript
[10, 100, 1000].reduce(function(accum, elem, index, arr){
    return accum + elem;
}, 0); // 1110
```

### Array.prototype.reduceRight() [nochg] [es5]
#### Array.prototype.reduceRight(func, [initial]) -> Object

Аналогично методу `Array.prototype.reduce`, выполняет вычисление справа налево.

### Array.prototype.indexOf() [nochg] [es5]
#### Array.prototype.indexOf(elem, [index]) -> Number

Выполняет поиск элемента в массиве и возвращает его индекс, если элемент не найден вернет `-1`.

Необязательный параметр `index` определяет элемент с которого начинается поиск.
По умолчанию поиск начинается с первого элемента.

### Array.prototype.lastIndexOf() [nochg] [es5]
#### Array.prototype.lastIndexOf(elem, [index]) -> Number

Аналогично методу `Array.prototype.indexOf`, выполняет поиск справа налево.
По умолчанию поиск начинается с последнего элемента.

### Array.prototype.toString()
