# TypeScript style sheet
Some small, usefull, real life examples]

- [Diference between types and interface](#diference-between-type-and-interface)
- [Partial types](#partial-types)
- [Required types](#required-types)
- [Pick types](#pick-types)
- [Omit types](#omit-types)
- [Inmutable types](#inmutable-types)
- [Conditionally assign types](#conditionally-assign-types)
- [Typeof tip](#typeof-tip)
- [Custom type string](#custom-type-string)
- [Type literals](#type-literals)
- [Interfaces](#interfaces)
- [Type assertions](#type-assertions)
- [Generics](#generics)
- [Enums](#enums)
- [Function](#functions)

### Diference between type and interface
interfaces are open and type aliases are closed. This means you can extend an interface by declaring it a second time.

```ts

interface BirdInterface {
  wings: 2;
}
//ALLOW
interface BirdInterface {
  wings: 10;
}

type BirdTypeYo = {
  wings: 2;
};
//NOT ALLOW
type BirdTypeYo = {
  wings: 2;
};

//ALLOW

interface BirdInterface extends Somthing {
  wings: 2;
}

```
### Partial types

```ts
type User = {
  name: string;
  age: number;
  gener: string;
};

type PartialUser = Partial<User>;

const user: PartialUser = {
  name: 'John Doe',
  age: 23,
};
```
### Required-types

```ts
type UserProfile = {
  description?: string;
  email?: string;
};

const userProfile: Required<UserProfile> = {
  email: 'my@email.com',
  description: 'My user descriptopn',
};

type Item = {
  name: string;
  price: number;
  description: string;
  currency: string;
  image: string;
};
```

### Pick types

```ts
type ItemPreview = Pick<Item, 'name' | 'image'>;

const item: Item = {
  name: 'Iphone',
  description: 'Model 10',
  price: 699,
  currency: 'USD',
  image: 'https://images.apple.com/...',
};

const itemPreview: ItemPreview = {
  name: 'Samsung',
  image: 'https://images.samsung.com/...',
};
```
### Omit types

```ts
type PricelessItem = Omit<Item, 'price' | 'currency'>;

const pricelessItem: PricelessItem = {
  name: 'Noikia',
  description: 'Model 11',
  image: 'https://images.nokia.com/...',
};
```

### Inmutable types

```ts
const car = {
  name: 'Ferrari',
  price: 50000,
} as const;

type Car = {
  readonly name: string;
  price: number;
};
```

### Conditionally assign types

```ts
type Tv = {
  name: string;
  brand?: string;
};

const tv: Tv = {
  name: 'Samsung',
};

const outOfStockTv: string = tv.brand!;
```

### Typeof tip

```ts
const player = {
  name: 'Sony',
  age: 18,
};

type Player = typeof player;

const player2 = {
  name: 'Bobby',
  age: 98,
};
```

### Custom type string

```ts
function handler(eventType: `on${string}`) {
  console.log(`handeling ${eventType}`);
}
```

### Type literals

```ts
let unit: string = 'px';
let miles: 'MILES' = 'MILES';
```

### Interfaces

Interfaces describe the shape of the value, but don't contains definitions of the properties

```ts
interface AttackFunction {
  (opponent: {alias: string; health: number}, attactWith: number): number;
}

interface OptionalAttributes {
  strength?: number;
}

interface ComicBookCharacter extends OptionalAttributes {
  secretIdentity?: string;
  alias: string;
  health: number;
  attack: AttackFunction;
}

function attackFunc(opponent, attackWith) {
  opponent.health -= attackWith;
  console.log(
    `${attackWith.alias} attacked ${opponent.alias}, who's health = ${opponent.health}`,
  );
  return opponent.health;
}
```
More
```ts
let superHero: ComicBookCharacter = {
  alias: 'She-Hulk',
  health: 5000,
  strength: 5000,
  attack: attackFunc,
};

let superVillain: ComicBookCharacter = {
  secretIdentity: 'Jack Napier',
  alias: 'Joker',
  health: 75,
  attack: attackFunc,
};

function getSecretIdentity(character: ComicBookCharacter) {
  if (character.secretIdentity) {
    console.log(`${character.alias} is ${character.secretIdentity}`);
  } else {
    console.log(`${character.alias} has no secret identity`);
  }
}
superHero.attack(superVillain, 100);
```

### Type assertions

```ts
interface SuperHero {
  powers: string[];
  savesTheDay: () => void;
}

let dazzler: SuperHero = {
  powers: ['traduces sonic vibrations into light'],
  savesTheDay: () => {
    console.log(`${dazzler.powers[0]}!`);
  },
};

interface BadGuy {
  badDeeds: string[];
  getRandomBadDeed: () => string;
  commitBadDeed: () => void;
}

let badGuy: BadGuy = {
  badDeeds: ['attempted toPhase the universe'],
  getRandomBadDeed: () =>
    badGuy.badDeeds[Math.floor(Math.random() * badGuy.badDeeds.length)],
  commitBadDeed: () => {
    console.log(`Bad guy ${badGuy.getRandomBadDeed()}`);
  },
};

function saveDayOrBadDeed(hero: SuperHero | BadGuy) {
  if ((hero as SuperHero).powers) {
    (hero as SuperHero).savesTheDay();
  } else {
    (hero as BadGuy).commitBadDeed();
  }
}

console.log(saveDayOrBadDeed(dazzler));
console.log(saveDayOrBadDeed(badGuy));
```
### Generics

```ts
function pushSomethingIntoCollection<T>(something: T, collection: T[]) {
  collection.push(something);
  console.log(collection);
}

let jeanGrey = {name: 'Jean Grey'};
let wolverine = {name: 'Wolverine'};

let superHereoes = [jeanGrey];
let powers = ['telepathy', 'teleportation'];

interface SuperHero1 {
  name: string;
}

pushSomethingIntoCollection(wolverine as SuperHero1, superHereoes);
pushSomethingIntoCollection('Radom text', powers);
```

## Enums

A not a type-level extension checking, they allow you to define a set of named constants. Using enums can make it easier to document intent, or create a set of distinct case.

```ts
enum Num {
  UNO = 1,
  TWO,
  THREE,
}

enum NumString {
  UNO = 'UNO',
  TWO,
  THREE,
}//This will ask you to initialize the rest of the values

function getNum(number: Num) {
  return number
}

getNum(Num.Uno);

enum Direction {
  Up = "UP",
  Down = "DOWN",
  Left = "LEFT",
  Right = "RIGHT",
}

```

### Functions

Few examples with Inference and Constrains

```ts
function something<T>(arr: T[]) : T | undefined {
  return arr[0]
}

function takeThis(arr: string[]): number[] {
  return arr.map((x: string) => Number(x));
}

function takeThisYes<Input, Output>(arr: Input[], func: (arg: Input) => Output): Output[] {
  return arr.map(func);
}

function longest<T extends {length: number}>(a: T, b: T): T {
 if(a.length > b.length) {
  return a
 }
  return b
}

longest('2', '10')
longest(2, '10') // This throws an error as number does not have a length
longest(['2'], ['10', '19'])

function concat<T>(arg1: T[], arg2: T[]): T[] {
  return arg1.concat(arg2)
}

concat([1,2], [3,4])
concat([1,2], ['3', '4']) // This throws an error as we want both arrays to have the same type

```

### void

void represents the return value of functions which don’t return a value. It’s the inferred type any time a function doesn’t have any return statements, or doesn’t return any explicit value from those return statements:

*void is not undefined

```ts
// The inferred return type is void
function noop() {
  return;
}
```

### never
Some functions never return a value:


```ts
// The inferred return type is void
function fail(msg: string): never {
  throw new Error(msg);
}
```
