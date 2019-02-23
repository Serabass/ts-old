# 7. @декораторы 

**Декораторы** - одно из нововведений ESNext.
По сути своей декоратор - это обычная функция, которая принимает от одного до трёх аргументов (в зависимости от типа декоратора).


## Немного воображения
Возможно это не совсем удачная аналогия, но давайте представим, что класс - это новогодняя ёлка, а декораторы - это самые разные игрушки, которыми мы обвешиваем нашу ёлочку).

## Примеры
В Спецификации TS есть 4 вида декораторов, для которых прописаны тайпинги:

 * [ClassDecorator](#classdecorator)
 * [MethodDecorator](#methoddecorator)
 * [PropertyDecorator](#propertydecorator)
 * [ParameterDecorator](#parameterdecorator)

## ClassDecorator
Объявление ([lib.es5.d.ts:1379](https://github.com/Microsoft/TypeScript/blob/v3.3.1/lib/lib.es5.d.ts#L1379)):
```typescript
declare type ClassDecorator = <TFunction extends Function>(target: TFunction) => TFunction | void;
```

Из названия понятно, что этот декоратор можно вешать только на классы. В target будет передана функция-конструктор этого класса.

### Экзампель:
```typescript

function Y(target: Function) {
    // do something
}

@Y
class X {

}

```

Аналогичный пример, только без использования декораторной функции как декоратора:
```typescript
class X {

}

Y(X);

```

## MethodDecorator
Объявление ([lib.es5.d.ts:1381](https://github.com/Microsoft/TypeScript/blob/v3.3.1/lib/lib.es5.d.ts#L1381)):
```typescript
declare type MethodDecorator = <T>(target: Object, propertyKey: string | symbol, descriptor: TypedPropertyDescriptor<T>) => TypedPropertyDescriptor<T> | void;
```

Вот здесь чуть-чуть веселее. Эту разновидность декораторов можно вешать только на методы (присутствие тела метода обязательно, иначе это не метод, а свойство, даже если содержит в себе функцию.). В аргументы приходит следующее:

1. Если метод статический, то в `target` передаётся функция-конструктор, как в примере выше. Если же метод инстансовый, то в `target` прилетит объект-прототип (`Class.prototype`) используемого класса.
2. Во второй аргумент `propertyKey` прилетит строка с именем поля. Эта строка генерируется во время компиляции кода в JS и не страдает, например, при минификации кода.
3. Ну и в третий аргумент `descriptor` будет передан дескриптор поля с указанным методом, полученный путём вызова `Object.getOwnPropertyDescriptor(target, propertyKey)`.

### Ехампле:
```typescript

function Y(target: Object, propertyKey: string | symbol, descriptor: PropertyDescriptor): PropertyDescriptor {
    descriptor.value = function () {
        console.log('Noooooo!');
    };

    return descriptor;
}

class X {
    @Y
    public myAwesomeMethod() {
        console.log('Luke, I\'m your father!');
    }
}

new X().myAwesomeMethod(); // Logs 'Noooooo!'
```
Данный пример показывает, как подменить ~~ребёнка в роддоме~~ метод у класса при помощи декоратора.

Ну и, конечно же, приведу пример кода  без использования декоратора:

```typescript

function Y(target: Object, propertyKey: string | symbol, descriptor: PropertyDescriptor): PropertyDescriptor {
    descriptor.value = function () {
        console.log('Noooooo!');
    };

    return descriptor;
}

class X {
    public myAwesomeMethod() {
        console.log('Luke, I\'m your father!');
    }
}

X.prototype['myAwesomeMethod'] = Y(X.prototype, 'myAwesomeMethod', Object.getOwnPropertyDescriptor(X.prototype, 'myAwesomeMethod') as PropertyDescriptor).value;

new X().myAwesomeMethod(); // Logs 'Noooooo!'
```

Примерно это и происходит, когда мы вешаем декоратор на метод. Только там, скорее всего, не присвоение, а вызов `defineProperty` с передачей обновлённого дескриптора.

## PropertyDecorator
Объявление ([lib.es5.d.ts:1380](https://github.com/Microsoft/TypeScript/blob/v3.3.1/lib/lib.es5.d.ts#L1380)):
```typescript
declare type PropertyDecorator = (target: Object, propertyKey: string | symbol) => void;
```

Поведение этих декораторов во многом схоже с `MethodDecorator`, только в этом случае (почему-то!) не передаётся дескриптор.
`В процессе`

## ParameterDecorator
Объявление ([lib.es5.d.ts:1382](https://github.com/Microsoft/TypeScript/blob/v3.3.1/lib/lib.es5.d.ts#L1382)):
```typescript
declare type ParameterDecorator = (target: Object, propertyKey: string | symbol, parameterIndex: number) => void;
```

`В процессе`

## Использование
Наверняка, вы уже задались вопросом: *"Хорошо, а как мне вызвать декоратор с какими-либо аргументами?"*. Это правильный вопрос.
Чтобы декоратор умел принимать аргументы, его нужно построить таким образом, чтобы функция-декоратор возвращалась из другой функции (я просто мастер объяснений).

```typescript
function Y(options: any): any {
    return function (target: any, propertyKey: string | symbol) {
        // Do something...
    };
}

class X {
    @Y({
        myOption: 'myValue',
    })
    public prop: number = 0;
}
```

## Немного лирики
Декоратор - это то, что в `Java` называется аннотацией, а в `C#` - аттрибутом. А, к слову, `Python` называет эту конструкцию точно так же - Декоратор. Можно предположить, что Python и послужил источником вдохновения для разработчиков спецификации `JS`/`TS`. Я не говорю на змеином, поэтому могу где-то ошибаться.

Декораторы используются только в связке с классами, их свойствами и методами. Это значит, что на свободно объявленную функцию/переменную/константу декоратор повешать нельзя. Это исключительно ООПэшная штуковина.

Не раз слышал мнение, что декораторы не нужны. Обычно, в таких случаях я отвечаю *"Вы просто не умеете их готовить"*, несмотря даже на то, что сама по себе плюшка ещё не совсем стабильная и до сих пор находится в состоянии `experimental`. Однако опыт ёлочки Angular2+ показал нам, что игрушки всё-таки не китайские).

## Пробуем на деле
Чтобы можно было использовать декораторы в вашем проекте, необходимо добавить опцию `"experimentalDecorators": true` в `tsconfig.json` вашего проекта. Иначе и IDE, и компилятор будут злобно на вас смотреть.

## Особенности использования декораторов
Стоит отметить, что порядок выполнения декораторных функций - снизу вверх, а не сверху вниз, как мы изначально ожидаем. А в случае с Параметровыми декораторами - справа налево, т.е. от последнего к первому.
Что же касается всего набора, то их порядок выполнения выглядит так:
 * ParameterDecorator
 * MethodDecorator
 * PropertyDecorator
 * ClassDecorator

`TODO: добавить пример с console.log();`

## Развитие
Декораторы, как и сам Typescript получили хороший пинок в развитии благодаря команде Angular. Именно там в полной мере декорируются компоненты, сервисы, pipes и тому подобные плюшки. Изначально Google пытался разработать свой `AtScript` (до которого я, к сожалению так и не добрался в своё время), а потом в какой-то момент они сказали *"Вот блять..."* и команда Angular взяла `Typescript` как основной язык для своей платформы.

## emitDecoratorMetadata
Эта довольно полезная опция заставляет компилятор оставлять в коде такую мета-информацию о классах/свойствах/методах/параметрах, как:
 * **`design:returntype`** - возвращаемый тип (для методов).
 * **`design:type`** - тип свойства (для свойств).
 * **`design:paramtypes`** - массив с типами аргументов (для методов).
 * ...

Говорят, появление этой плюшки спровоцировала команда Angular со своим Dependency Injection и параллельно с этим появилась [вот такая разработка](https://github.com/typestack/typedi).

Как работать с метаданными через **Reflect**, я опишу [в другой статье](../6.%20Reflect)

В качестве хорошего примера привожу библиотеку [ngx-resource-core](https://github.com/troyanskiy/ngx-resource-core), которая построена на использовании декораторов в сервисах Angular.

## TODO
1. Использование декораторов как обычных функций - где и зачем.
