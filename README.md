# Linked Helper for LinkedIn front-end interview questions

Это первый этап - интервью с HR. В конце считают процент решенных задач и если он ниже определенного уровня, то прощаются.

### Вопрос 1. Что будет выведено в консоль?

```js
(function question1() {
  const f0 = () => {
    console.log(0);
    return 0;
  };
  const f1 = () => {
    console.log(1);
    return 1;
  };
  const f2 = () => {
    console.log(2);
    return 2;
  };
  const f3 = () => {
    console.log(3);
    return 3;
  };
  const f4 = () => {
    console.log(4);
    return 4;
  };

  console.log(f0() || (f1() && f2()) || f3() & f4());
})();

// Ответ 0 1 2 2
```

### Вопрос 2.

Код воспроизвел по памяти.

```js
(function question2() {
  const person = {
    name: "Вася",
  }(
    // Вроде так у них было но оно не работает
    function func(person) {
      person = {
        name: "Петя",
      };
    }
  )(person);

  // Вот так у меня работает
  // function func(person) {
  //   person = {
  //     name: "Петя"
  //   }
  // }
  // func(person)

  console.log(person.name);
})();

// Ответ был Вася. Дополнительный вопрос был что изменится если поменять const на let. Ответил что ничего.
```

### Вопрос 3. Что будет выведено в консоль?

```js
(function question3() {
  try {
    let x = 4;
    if (true) {
      console.log("x_let:", x);
      let x = 5;
    }
  } catch (err) {
    console.log("x_let:error");
  }

  try {
    let y = 4;
    if (true) {
      let y = 5;
      console.log("y_const_1:", y);
    }
    console.log("y_const_2:", y);
  } catch (err) {
    console.log("y_const_1:error");
  }
})();

// Ответ x_let:error -> y_const_1: 5 -> y_const_2: 4
```

### Вопрос 4. Что будет выведено?

```js
function makeGroup() {
  let people = [];

  let i = 0;
  while (i < 10) {
    let man = function () {
      alert(i);
    };
    people.push(man);
    i++;
  }

  return people;
}
let group = makeGroup();

group[0]();
group[5]();

// Ответ 10 10
```

### Вопрос 5. Что выведется в консоль?

```js
(function question5() {
  let baz = 0;

  let foo = {
    bar1: function () {
      return this.baz;
    },
    bar2: () => this.baz,
    baz: 2,
  };

  let fooCopy = {
    bar1: foo.bar1,
    bar2: foo.bar2,
    baz: 2,
  };

  console.log(fooCopy.bar1());
  console.log(fooCopy.bar2());
})();

// Ответ: 2 -> undefined(или ошибка в strict mode)
```

### Вопрос 6. Какие номера строк будут выведены и в каком порядке

```js
(function question6() {
  const p = new Promise((_, reject) => {
    setTimeout(() => {
      console.log("reject");
      reject();
    }, 2000);
  });

  p.then(
    () => console.log("10"),
    () => console.log("11")
  ).then(
    () => console.log("13"),
    () => console.log("14")
  );

  p.then(
    () => console.log("18"),
    () => console.log("19")
  );

  p.then(
    () => console.log("23"),
    () => console.log("24")
  );
})();

// Ответ: reject -> 11 -> 19 -> 24 -> 13

// Упрощенная версия прошлой задачи для тех кто ответил не правильно
(function question6() {
  const p = Promise.reject();

  p.then(() => console.log("6"))
    .catch(() => console.log("9"))
    .then(() => console.log("11"));

  p.then(() => console.log("16"))
    .catch(() => console.log("19"))
    .then(() => console.log("21"));
})();

// Ответ: 9 -> 19 -> 11 -> 21
```

### Вопрос 7. Что выведется в консоль?

```js
(function question7() {
  class Fruit {}

  Object.assign(Fruit.prototype, {
    color: "red",
    names: [],
    addName(name) {
      this.names.push(name);
    },
  });

  const fruit1 = new Fruit();
  const fruit2 = new Fruit();

  fruit1.color = "yellow";

  fruit1.addName("banana");
  fruit1.addName("coconut");

  console.log("20:", fruit1.color, fruit2.color);
  console.log("21:", fruit1, fruit2, fruit1.names, fruit2.names);

  delete fruit2.names;
  console.log("24:", fruit2.names, Fruit.prototype.names);

  fruit2.names = [];
  fruit2.addName("apple");

  console.log("29:", fruit1.names, fruit2.names);

  console.log("31:", Fruit.prototype);
  console.log("32:", Fruit.prototype.constructor);
  console.log("33:", Fruit.prototype.constructor.prototype);
  console.log("34:", Fruit.prototype.constructor.prototype.protope); //protope в задаче. Это не моя опечатка.
})();

// Ответы
// 20: yellow red
// 21: ['banana', 'coconut'], ['banana', 'coconut']
// 24: ['banana', 'coconut'], ['banana', 'coconut']
// 29: ['banana', 'coconut'], ['apple']
// 31: {color: 'red', names: ['banana', 'coconut'], addName: ƒ}
// 32: class Fruit {}
// 33: {color: 'red', names: ['banana', 'coconut'], addName: ƒ}
// 34: undefined
```

### Задача 8.

Воспроизвожу по памяти

```js
function Example() {
  const [counter, setCounter] = useState(0);

  useEffect(() => {
    const id = setInterval(() => {
      setCounter(counter + 1);
      console.log(counter);
    }, 1000);

    return ()=>clearInterval(id)
  }, []);

  // Точно не помню код, но суть в том что useEffect запускается 1 раз и выводит 0,1,1,1,1,1,1 ...
  // Ответ 0,1,1,1,1,1,1 или 0,1 и дальше ничего для старых версий React
}
```

### Задача 9. Что будет выведено в консоль?

```js
function Example() {
  const [flag, setFlag] = useState(false);

  useEffect(() => {
    setFlag(true);

    console.log('effect run');

    return ()=>console.log('effect clean up')
  }, [flag]);

  console.log('render');

  return null;
  // Ответ render -> effect run -> render -> effect clean up -> effect run -> render
}

