Строки
======

Строка это неизменяемая, упорядоченная последовательность 16-битных значений, каждое из которых представлено символом Юникода.

Нумерация символов в строке начинается с нуля.

Синтаксис
---------

### Создание

Строковый литерал:

    var foo = 'строка текста',
        bar = "строка текста",
        spam = new String('строка текста');
        
Многострочный текст:
    
    // ES3
    var text = 'первая строка\n' +
               'вторая строка\n' +
               'третья строка\n';
               
    // ES5
    var text = 'первая строка\n\
                вторая строка\n\
                третья строка\n';

### Суррогатные пары

Некоторые символы могут содержать более 16-бит и будут представлены суррогатной парой из двух 16-битных значений. Такая строка состоящая из 1 символа(суррогатной пары) будет иметь длину 2, т.к. методы JS оперируют не символами, а 16-битными значениями.

### Управляющие последовательности в строковых литералах

    var uni = '\u044a', // символ 'ъ'
        lat = '\x77'; // символ 'w'

| Последовательность | Символ        |
|--------------------|---------------|
| \t | Горизонтальная табуляция (\u0009) |
| \v | Вертикальная табуляция (\u000B) |
| \n | Перевод строки (\u000A) |
| \r | Возврат каретки (\u000D) |
| \" | Двойная кавычка (\u0022) |
| \' | Одинарная кавычка (\u0027) |
| \\ | Обратный слэш (\u005C) |
| \xXX | Символ Latin-1, заданный двумя 16-ричными цифрами XX |
| \uXXXX | Символ Unicode, заданный четырьмя 16-ричными цифрами XXXX |

Свойства
--------

### String.prototype

Прототип.


### String.prototype.length

Количество 16-битных значений.


### String.prototype.constructor

Ссылка на функцию-конструктор String.

Методы
------

### String.fromCharCode(charCode, ...) -> String

Создает строку символов из целочисленных кодов соответствующих символам Юникода.

### String.prototype.charAt(pos) -> String

Возвращает символ находящийся в строке в указанной позиции.

    var text = 'My text';
    text.charAt(0); // 'M', первый символ строки
    text.charAt(text.length-1); // 't', последний символ строки
    
### String.prototype.charCodeAt(pos) -> Number

Возвращает числовой код символа находящегося в строке в указанной позиции. Возвращаемое значение целое число между 0 и 65535.

### String.prototype.concat(str, ...) -> String

Выполняет конкатенацию строк и возвращает новую строку. Конкатенацию строк также можно выполнить с помощью оператора `+`.

### String.prototype.indexOf(str, [start]) -> Number

Выполняет поиск подстроки в строке, начиная с начала. Возвращает позицию первого вхождения подстроки `str` в строку, начиная с позиции `start`, если подстрока найдена, или `-1`, если не найдена.

### String.prototype.lastIndexOf(str, [start]) -> Number

Выполняет поиск подстроки в строке, начиная с конца. Возвращает позицию первого вхождения подстроки `str` в строку, начиная с позиции `start`, если подстрока найдена, или `-1`, если не найдена.

### String.prototype.localeCompare(str) -> Number

Сравнивает строки с учетом порядка следования символов национальных алфавитов.

### String.prototype.match(regexp) -> Array | null
Находит одно или более соответствий регулярному выражению. Если в `regexp` установлен флаг `'g'`(глобальный поиск), будут найдены все соответствия строки регулярному выражению. Если флаг `g` не установлен, то будет найдено первое соответствие строки регулярному выражению. Свойство `index` массива будет указывать на 
