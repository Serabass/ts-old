# Поговорим о Typescript (и не только)

Здесь будет цикл статей, посвящённых основным особенностям и приятностям языка [Typescript](http://www.typescriptlang.org/).

**Typescript (далее - TS)** - язык, разработанный Microsoft. Является надстройкой (superset) над Javacript (далее - JS). Почти всегда обычный JS - является валидным TS.

В этом цикле статей я собираюсь описать особенности этого языка, его преимущества (большинство из которых постепенно кочуют в оригинальный JS).
Отмечу с порога, что я *не буду* приводить никаких сравнений с другими аналогичными продуктами, такими как `Flow/Flowtype`,
`Coffeescript`, `Livescript` и прочими ~~ужасными~~ решениями. Это не имеет смысла, т.к. моя задача поделиться знаниями конкретного языка.
Также я буду затрагивать те вещи, которые ровно так же относятся к JS. Это будут функциональные нововведения вроде
`Proxy`, `Observable`, `Reflect` и им подобные.

О чём будет цикл статей (пока что набрасываю хаотично, позже наведу порядок):
1. [Введение](./1.%20Intro)
2. Объектно-ориентированный подход
    1. Классы
    2. Интерфейсы
    3. Generics
    4. Абстрактные классы
3. Conditional Types
4. Proxy
5. [Promise и магия `async`/`await`](./5.%20Promise)
6. [Reflect](./6.%20Reflect)
7. [@декораторы](./7.%20Decorators)
8. [Стрелочные функции](/8.%20Arrow%20functions/)
9. Декларации типов
    1. Хранилища тайпингов (DefinitelyTyped, @types, etc)
10. Observable
11. Symbols
12. [Template Strings](./12.%20Template%20Strings)
13. [Что такое Source Maps, зачем они нужны и как они работают?](./13.%20Source%20Maps)
14. `let` и `const`
15. [Полезные сниппеты](./15.%20Useful%20snippets)
16. Деструктуризация объектов и массивов
17. Генераторы


Предлагайте в `Issues` какую тему ещё можно раскрыть в этом цикле.
