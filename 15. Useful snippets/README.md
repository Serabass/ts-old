# 15. Полезные сниппеты

Захотелось добавить такой документ, куда я буду складывать интересные, полезные и просто какие угодно блоки кода. Просто чтобы лежали.

## 1. Более лаконичная замена Object.keys(...).forEach
```typescript
let o = {
    a: 1,
    b: 2
};

Object.entries(o).forEach(([key, value]) => {
    console.log(key, value);
});

// А если, например, нам нужен только value

Object.entries(o).forEach(([, value]) => {
    console.log(value);
});

// a 1
// b 2
```
