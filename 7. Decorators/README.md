# @декораторы 

**Декораторы** - одно из нововведений ESNext.
По сути своей декоратор - это обычная функция, котоая принимает от одного до трёх аргументов.
В Спецификации TS есть 4 вида декораторов, для которых прописаны тайпинги:

 * [ClassDecorator](#classdecorator)
 * [MethodDecorator](#methoddecorator)
 * [PropertyDecorator](#propertydecorator)
 * [ParameterDecorator](#parameterdecorator)

## ClassDecorator
Объявление:
```typescript
declare type ClassDecorator = <TFunction extends Function>(target: TFunction) => TFunction | void;
```


## MethodDecorator
Объявление:
```typescript
declare type MethodDecorator = <T>(target: Object, propertyKey: string | symbol, descriptor: TypedPropertyDescriptor<T>) => TypedPropertyDescriptor<T> | void;
```


## PropertyDecorator
Объявление:
```typescript
declare type PropertyDecorator = (target: Object, propertyKey: string | symbol) => void;
```


## ParameterDecorator
Объявление:
```typescript
declare type ParameterDecorator = (target: Object, propertyKey: string | symbol, parameterIndex: number) => void;
```

Декоратор - это то, что в `Java` называется аннотацией, а в `C#` - аттрибутом. А, к слову, `Python` называет эту конструкцию точно так же - Декоратор. Можно предположить, что он и послужил источников вдохновения для разработчиков спецификации `JS`/`TS`.

Декораторы используются только в связке с классами, их свойствами и методами. Это значит, что к свободно объявленной функции декоратор применить нельзя.

## Пробуем на деле
Чтобы можно было использовать декораторы в вашем коде, необходимо добавить опцию `"emitDecoratorMetadata": true` в `tsconfig.json` вашего проекта.

## Особенности использования декораторов
Стоит отметить, что порядок выполнения декораторных функций - снизу вверх, а не сверху вниз, как мы изначально ожидаем. А в случае с Параметровыми декораторами - справа налево, т.е. от последнего к первому.
Что же касается всего набора, то их порядок выполнения выглядит так:
 * ParameterDecorator
 * MethodDecorator
 * PropertyDecorator
 * ClassDecorator

TODO: Привести примеры использования декораторов с аналогичными блоками кода без испольнования Д.
TODO: Привести пример ngx-resource
TODO: Рассказать об emitDecoratorMetadata
TODO: Использование декораторов как обычных функций - где и зачем.


