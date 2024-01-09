---
level: easy
tags:
  - typescript
  - mapping
linkToTask: https://typehero.dev/challenge/day-11
---
## Subscription

*разговор двух эльфов, Вунорса и Алебастра, в мастерской Санты в понедельник утром после того, как все эльфы были вынуждены работать все выходные*

*Вурнос* Мир не узнает мира до тех пор, пока каждый консультант, участвующий в продвижении SOC2, не умрет и не окажется в земле.

*Алебастр* Тяжелые выходные?

*Вурнос* Да. И теперь я только что получил известие от начальства, что мне нужно сделать типы всей нашей кодовой базы доступными только для чтения. DEEP только для чтения.

*Алебастр* Ой. Но, по крайней мере, так будет лучше, потому что что-то в чем-то превосходство функционального программирования что-то в этом роде..

*Вурнос* Дело не в этом. Дело в том, что ТОЧКИ НЕТ. Они говорят, что это требование SOC2. К SOC2 это не имеет ни малейшего отношения. На данный момент это буквально театр безопасности. А мы марионетки.

*Алебастр* Давайте попробуем использовать это как предлог для переноса кодовой базы на Rust, где по умолчанию все неизменяемо.

*Wurnose* Я это уже пробовал. Они сказали, что Rust слишком новый и непроверенный. Однако они рассматривают возможность Go.

*Алебастр* Лол дафук? Rust старше Go примерно на 3 года. Хорошо. Извините, что спросил. Тогда давайте реализуем DeepReadonly!

### API типов

SantaListProtector принимает произвольный тип в качестве входных данных и изменяет этот тип, чтобы он был доступен только для чтения. Хитрость здесь в том, что это должно работать и для любых вложенных объектов.

Пример кода:

```typescript
type test_0_actual = SantaListProtector<{
  hacksore: () => 'naughty';
  trash: string;
  elliot: {
    penny: boolean;
    candace: {
      address: {
        street: {
          name: 'candy cane way';
          num: number;
        };
        k: 'hello';
      };
      children: [
        'harry',
        {
          saying: ['hey'];
        },
      ];
    };
  };
}>;

type test_0_expected = {
  readonly hacksore: () => 'naughty';
  readonly trash: string;
  readonly elliot: {
    readonly penny: boolean;
    readonly candace: {
      readonly address: {
        readonly street: {
          readonly name: 'candy cane way';
          readonly num: number;
        };
        readonly k: 'hello';
      };
      readonly children: readonly [
        'harry',
        {
          readonly saying: readonly ['hey'];
        },
      ];
    };
  };
};

type test_0 = Expect<Equal<test_0_expected, test_0_actual>>;
```

---
## Solution

```typescript
type SantaListProtector<T> = {
  readonly [K in keyof T]: T[K] extends () => any ? T[K] : T[K] extends object ? SantaListProtector<T[K]> : T[K]
}
```

---
## Explanation

При помощи маппинга проходимся по объекту и, если T[K] - функция, то просто возвращаем T[K]. Иначе, если T[K] - объект, то рекурсивно вызываем этот же тип SantaListProtector<T[K]> только уже передаем T[K] в качестве дженерика. Иначе - выводим T[K].