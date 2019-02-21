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

