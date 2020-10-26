
## 1. Используйте = = =  вместо  = =

В JavaScript используются два вида операторов равенства: _===_  |  _!==_  и  _==_  |  _!= ._  Рекомендуется всегда использовать первый набор при сравнении (* предотвращает ошибки приведения типов. Здесь и далее примеч. пер.).

> «Если двое операндов имеют один и тот же тип и значение, то в результате использования === получаем true, а в результате использования !== – false.» – «JavaScript: The Good Parts» (* JavaScript. Сильные стороны).

Однако при работе с  _==_  и  _!=_  у вас могут возникнуть ошибки (* приведения типов), если значения имеют различный тип данных. В этих случаях они попробуют преобразовать тип значений, что часто приводит к ошибкам.


 фигурной скобки. Очевидно, что этот прием – ужасная практика, которую следует избегать во что бы то ни стало. Единственный случай, где стоило бы опускать фигурные скобки, – использование однострочников (* программа, эффект которой значительно больше, чем можно было бы предположить по её размеру), да и то, этот случай – очень спорный вопрос.

1

`if``(2 + 2 === 4)` `return` `'nicely done'``;`




## 2.  Размещайте ваш скрипт в конце страницы

Этот совет уже упоминался в предыдущей статье этой серии. Но поскольку это очень уместно и тут, то я повторюсь.

![Place JS at bottom](https://cdn.tutsplus.com/net/uploads/legacy/327_30Musts/javascriptButton.png)

Помните, что главная цель – сделать загрузку страницы максимально быстрой для пользователя. При загрузке скрипта браузер не может продолжить (* начать выполнение кода), пока не загружен весь файл. Поэтому пользователь должен будет ждать дольше, чтобы заметить какой-либо прогресс (* если скрипт размещается не в конце файла).

Если у вас имеются файлы JS, единственная цель которых – добавление какой-то функциональной возможности (например, после нажатия кнопки), то вперед, разместите эти файлы внизу, сразу перед закрывающим тегом body. Это безусловно относится к устоявшейся практике.

#### Более удачный вариант


`<p>And now you know my favorite kinds of corn. </p>`

`<script type``="text/javascript"` `src="path/to/file.js"></script>`

`<script type="text/javascript"` `src="path/to/anotherFile.js"></script>`

`</body>`

`</html>`

## 3. Объявляйте переменные снаружи от цикла For

При выполнении длинных циклов «for» не создавайте дополнительной нагрузки на движок. Например:

#### Неудачный вариант



`for``(``var` `i = 0; i < someArray.length; i++) {`

`var` `container = document.getElementById(``'container'``);`

`container.innerHtml +=` `'my number: '` `+ i;`

`console.log(i);`

`}`

Обратите внимание, как нам необходимо определять длину массива при каждой итерации и как мы каждый раз обходим DOM (* объектная модель документа) для получения элемента с id "container" – очень неэффективно!

#### Более удачный вариант



`var` `container = document.getElementById(``'container'``);`

`for``(``var` `i = 0, len = someArray.length; i < len; i++) {`

`container.innerHtml +=` `'my number: '` `+ i;`

`console.log(i);`

`}`

Кто оставит в комментариях советы по улучшению вышеуказанного блока кода, получит бонусные баллы.


## 4. Уменьшите количество переменных в глобальной области видимости

> «Уменьшая ваш след (* количество переменных) в глобальной области видимости до единственного имени, вы значительно снижаете шансы дурного взаимодействия с другими приложениями, виджетами или библиотеками.»  
> – Douglas Crockford.

`var` `name =` `'Jeffrey'``;`

`var` `lastName =` `'Way'``;`

`function` `doSomething() {...}`

`console.log(name);` `// Jeffrey -- or window.name`

#### Более удачный вариант


`var` `DudeNameSpace = {`

`name :` `'Jeffrey'``,`

`lastName :` `'Way'``,`

`doSomething :` `function``() {...}`

`}`

`console.log(DudeNameSpace.name);` `// Jeffrey`

Обратите внимание на то, как мы «уменьшили наш след» до всего лишь одного шутливо названного объекта – "DudeNameSpace".

## 5. Комментируйте свой код

Поначалу может показаться, что в этом нет необходимости, но поверьте мне, вам следует комментировать ваш код насколько это возможно. Что произойдет, когда вы вернетесь к вашему проекту через несколько месяцев, – только то, что вы поймете: не так-то просто вспомнить ход собственной мысли. Или что если одному из ваших коллег необходимо будет внести изменения в ваш код? Всегда, всегда комментируйте важные части вашего кода.


/ Cycle through array and echo out each name.`

`for`(``var` `i = 0, len = array.length; i < len; i++) {`

`console.log(array[i]);`

`}`



## 6. Не передавайте строку в "SetInterval" или "SetTimeOut"

Рассмотрим следующий код:

`setInterval(`

`"document.getElementById('container').innerHTML += 'My new number: ' + i"``, 3000`

`);`

Этот код не только неэффективен, но и ведет себя таким же образом, как и вела бы себя функция "eval".  **Никогда не передавайте строку в "SetInterval" или "SetTimeOut".**  Вместо этого передавайте имя функции.



```
setInterval(someFunction, 3000);
```


## 7. Используйте {} вместо New Object()

Имеется множество способов создания объектов в JavaScript. Вероятно, более традиционным способом является использование конструктора «new», например:

`var` `o =` `new` `Object();`

`o.name =` `'Jeffrey'``;`

`o.lastName =` `'Way'``;`

`o.someFunction =` `function``() {`

`console.log(``this``.name);`

`}`

Однако, этот способ зарекомендовал себя как «плохую практику» без должных на то оснований. Вместо этого я вам рекомендую использовать более надежный способ создания объектов – при помощи литерала (* литеральная (символьная) константа).

#### Более удачный вариант
```
var o = {

name:'Jeffrey',

lastName ='Way',

someFunction :function() {

console.log(this.name);

}

};
```
Обратите внимание, что если вы просто хотите создать пустой объект, использование {} решит проблему.

1

`var` `o = {};`

> «За счет использования литералов объектов мы можем писать код, при помощи которого реализуется множество возможностей и в то же время написание которого интуитивно понятно разработчику. При этом нет необходимости в вызове конструктора напрямую или соблюдении верной последовательности передаваемых в функцию аргументов и т.д.» –  [dyn-web.com](http://www.dyn-web.com/tutorials/obj_lit.php)

## 8. Используйте [] вместо New Array()

То же справедливо и при создании нового массива.

#### Нормально
```
var a =new Array();

a[0] ="Joe";

a[1] ='Plumber';
```
#### Более удачный вариант
```
var a = ['Joe','Plumber'];
```
> «Обычная ошибка в программах на JavaScript – использование объекта там, где необходим массив, или массива, когда необходимо воспользоваться объектом. Правило простое: когда именами свойств являются небольшие последовательные числа, вам необходимо использовать массив. В ином случае – объект.» – Дуглас Крокфорд.


## 9. Всегда, всегда используйте точки с запятой (;)

Технически в большинстве браузеров опускание «;» сойдет вам с рук.

`var` `someItem =` `'some string'`

`function` `doSomething() {`

`return` `something`

`}`

Тем не менее, это очень плохая практика, в результате использования которой потенциально могут возникнуть гораздо более крупные и тяжелые для обнаружения проблемы.

#### Более удачный вариант

`var` `someItem =` `'some string';`

`function` `doSomething() {`

`return` `'something';`

`}`


## 10. Функции-выражения немедленного вызова

Вместо того, чтобы вызывать функцию, можно довольно просто сделать так, чтобы она вызывалась автоматически при загрузке страницы или после вызова родительской функции. Просто оберните вашу функцию в круглые скобки и затем добавьте дополнительную пару, благодаря которой собственно функция и вызывается.

`(function doSomething() {`

`return` `{`

`name:` `'jeff',`

`lastName:` `'way'`

`};`

`})();`

