# TypeScript style sheet
Some small, usefull, real life examples]

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

//Interfaces describe the shape of the value, but don't contains definitions of the properties

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