# AMD

[AMD](https://github.com/amdjs/amdjs-api/wiki/AMD) расшифровывается как *Asynchronous Module Definition, определение асинхронного модуля*. Этот стандарт — альтернатива спецификации Common JS (CJS).

API описывает механизм для определения таких модулей, что и сам модуль, и его зависимости загружаются асинхронно. Это особенно хорошо подходит для браузерной среды, в которой синхронная по умолчанию загрузка модулей влияет вызывает проблемы с производительностью, юзабилити, отладкой и кроссдоменным доступом.

AMD-библиотеки предоставляют глобальную функцию `define` для определения модуля


```js
define(modulename?,[dependencyA?, dependencyB?, ...], function(objectA, objectB, ...) {
...
    var myExportedObj = function() {...}
    return myExportedObj;

});
```

Где

- `modulename` это необязательный строковый параметр, явно задающий идентификатор определяемого модуля
- `dependencyA`, `dependencyB` и так далее — зависимости определяемого модуля
- `function(objectA, objectB) {...}` — функция-фабрика, аргументами которой являются экспортированные объекты каждой из зависимостей
- `myExportedObj` — необязательное возвращаемое значение (модуль ведь может просто добавлять методы к существующему объекту и ничего не возвращать). Если какое-либо значение возвращается, то оно становится экспортируемым объектом своего модуля, и именно его получат другие модули, если укажут `modulename` в списке своих зависимостей.

Помимо глобальной функции `define` AMD-совместимая библиотека должна имеит свойство `define.amd`, значение которого — объект. Проверка `define` и `define.amd` на существование в глобальной области видимости позволяет любому скрипту определить, что он вызывается AMD-загрузчиком.

Примеры библиотек, реализующих загрузку AMD-модулей:

- [Require JS](http://requirejs.org/docs/whyamd.html), написана [Джеймсом Бёрком](https://github.com/jrburke/) из Mozilla. Одна из первых библиотек, получивших широкое распространение и всё ещё одна из самых популярных. Предоставляет ограниченную совместимость с CommonJS-модулями.
- [CurlJS](https://github.com/cujojs/curl), часть фреймворка [CujoJS](http://cujojs.com/). CurlJS менее популярна, чем RequireJS. С 2014 года её поддерживают в рабочем состоянии, но не добавляют никаких новых возможностей.
- [Alameda](https://github.com/requirejs/alameda), также написанная Джеймсом Бёрком. Похожа на RequireJS, но использует промисы для работы с результирующими событиями.
- [Cajon](https://github.com/requirejs/cajon) также написанная Джеймсом Бёрком. Можно считать её декоратором для RequireJS, заменяющим метод `load` для загрузки зависимостей через Ajax.
- [SystemJS](https://github.com/systemjs/systemjs), написана [Гаем Бедфордом](https://github.com/guybedford), который несколько лет назад был одним из  наиболее активных разработчиков плагинов для RequireJS. SystemJS может загружать модули AMD, CommonJS и ES6. Часто используется в связке с [jspm](http://jspm.io/) — менеджером зависимостей (наподобие [Bower](Bower.md)), подгружаемых с Github и NPM.

Все эти библиотеки позволяют разработчику запустить проект без сборки, подгружая все зависимости асинхронно. Однако, собирать или упаковывать все модули в один файл для «боевого» окружения всё же рекомендуется — это уменьшит вес кода и количество запросов к серверу, а значит увеличит скорость загрузки страницы. Говорят, что поддержка браузерами и серверами стандарта [HTTP2](https://http2.github.io/) устранит необходимость совершения дополнительных запросов при асинхронной подгрузке зависимостей, так что и потребность в сборке проекта тоже будет устранена.

Пример других библиотек, которые не могут загружать зависимости асинхронно, но могут включать AMD-модули в свой процесс сборки:

- [Webpack](WEBPACK.md)
- [Rollup](http://rollupjs.org/)
- [StealJS](http://stealjs.com/)
