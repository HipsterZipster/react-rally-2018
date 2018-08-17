# React Rally 2018 SLC Utah - 20180916

# Shirley wu - React + D3 = Data Viz

1.  list attributes
    categorical categority
    quantitivate
    temporal - released
    spatial

2.  Ask questions

Picking a few "interesting" attributes
released
metascore
box office

when are most blockbusters released? inth e summer thanks and christmas?

what is the distribution of metasscores?
do blockbusters score high?

3.  explore data visually
    d3
    vega-lite
    python (matplotlib)
    r (ggplot2)
    tableau

## bar chart - categorical distributions

when are the most blockbuster released?

## histogram - quantitative distrubitions

what is the distrubtion of metascores?

## scatterplot - correlations

do movies that do well in the bo office tend to have higher metascores

4.  Design with a message in mind

blockbuser movies are highly seasonal
the highest box office figures were during the holidays

5.  assign visual encoding
    markets and channels

map invidivual or aggregate data elemtns rto marks

map data attributes to cannels

marks are these gometris shapes like lines
channels - xy position, width/hieght radius
sequential and divergent color schemes

# Breakdown

- mark - movie

- channels
  release date x position
  box office - y position
  metascore - color

- check out d3 area generator

## Frontend masters courses by Shirley

- data visualzaition for react developers
- introduction to d3
- building custom data visualaizations

## Books

- "The functional art" by alberto

# Sunil Pai - Front end engineer at Oculus

Where i speak to 20 minutes about one of th eboring bits of the job but it's important

"Oculus Rooms"

Oculus VR demo using React Components playing chess

"Oculus Venues" - demo also made using react js completely

- `v = f(state)`
  where f = components

- `nextState = f(state, action)`
  where
  f = reducers

- What do product requirements look like?
- Who writes all the bits that fire actions

## The something statements

- something hpapened
- wait til something happens
- do something
- do something in parallel `fork(async)`
- pay attention to something else that happening `join(task)`
- stop doing something `task.cancel()`
- read something from the moduel `select(x => x.path))`

product requirements for "nearby avatars"

- on joined / left / changed run "set-udpates" and breadcast block/mute sattuses
  0 on reconnectoin reset nearby avatar

## seat-update

store in state
if a friend joined your pod, broadcast

## menus

- if no wifi/4g, blick till wifi comes back
- if avatar sisn't set up, block on the avatar setup screen
- show facebook login screen
- get permissions for social sharing prefs (show once)
- show the venues list **(unless deeplinked into a specific venue)**
- show the venues details
- show the CoC video at least once per session
- if all good, enter venue

# Why React is _not_ Reactive - Shawn Swyx Wang

slides.com/swyx/why-react-is-not-reactive/live

Functional-reactive libraries like RxJS make it easy to understand how data changes, giving us tools to declaratively handle events and manage state. But while our render methods react to state changes, React isn’t reactive. Instead, we write imperative event-handlers, and trip up on gotchas like async setState and race conditions. Why? In this talk we build a Reactive React to show the difference between the "push" and "pull" paradigms of data flow and understand why React chooses to manage Scheduling as a core Design Principle, enabling awesome features like async rendering and Suspense!

## requirement comes up - "Make this request cancellable"

how it feels to learn rxjs with react js
this talk is not a full intro to:

- reactive programming
- intro to funtional reactive programming

Everyone knows spreadsheets
What so great about spreahssets
minimum viable app (db + ui)
only business lgoic, 100% declarative
nlcuindg update logic
rendering updates dependecnies

<Angular />

javascript is not reactive - data depedencies dont update calculations
but it has reactive apis

`btn.addEventListener(()=> clog('clicked'))`

observables = podcasts

Everything is or can be a stream

- dom event, web sockets, server events, setInterval, node streams...

viewstream <= datastream
v$ = f(d$)

WHy doesn't react use streams?

there's an internal joke in the react team that it should just be called "schedule"

What if react was reactive?

```jsx
class Counter extend Component {
  initialState = 0;
  increasment = createHandler(() => 1)
  // ...
}
```

https://reactive-react.netlify.com/

why isnt react reactive?

- too many updates - we need to batch stuff
- push vs pull mental model

- push whenever we want into the queue
- pull whenever want to render a frame

## Scheduler

- React ui virtial machine -> fiber scheudler -> react runtime system

- not just a passive render layer
- enqeueUpdate
- browser reduce operating system to a poorly
  -- react is virtual operation that reduces browsers to a render target for your apps

veiew runtime = F\*(data stream)

v = f*(d$)
f* = fiber schedule

push + pull++
time sliced rendering?

read `ReactUpdateQueue.js`

WHo writes all the bits that fire actions?

- Today's react needs imperative side effects
- What if we can pull it into a separate render function
- ReactFiberReconsiler.js - 300 loc

# Algebraic effects, Fibers, Coroutines...Oh my! - Brandon Dail (React Core)

React Fiber was a full re-write of React that will enable new and exciting patterns around control flow, which we've seen previewed with React Suspense. But what is a fiber? How does it relate to a coroutine? What are algebraic effects, and why do I keep hearing about them? This talk will go over these computer science topics in the context of React Fiber, to help shed some light on how React Fiber is implemented and the control flow concepts behind the new APIs.

## React Fiber - API compatibile rewrite of react

### Motivations

- Async Rendering - time slicing and suspense
- Gives react more low-level control over progarm execution

#### Time Slicing

- Lets react keep the page responsive where it matters
- React can prioritize time0senstive ineractoins like typing into a text input so that less time senstive updates like updating a long list, dont interrupt the user experience

#### Suspense

A generic way for an part of your app to pause rendering while it gets some external data

Common Requirement is "interrupting" so that React can pause the renderer and then resume it

Why do we need Fiber for that though? Can't we just add a flag?

Stack Frames

```js
let frame: Frame = {
  return: Frame,
  fn: add,
  parameters: [2, 2],
  localVariables: {
    result: 4
  }
};
```

Fiber

```js
let fiber: Fiber = {
  return: Fiber,
  component: Avatar,
  parameters: [2, 2],
  localVariables: {
    result: 4
  }
};
```

Fiber: A React Call Stack that has specific control over what needs to be done

#### The Scheduler

Since react doesnt rely on the call stack for keeping track of th ework it's doing, it has its own scheudler

unlike stack frame, a fiber can keep local data like state even if it gets interrupted, there also better at working together. It can preserve a lot more information and more intelligently prioritize other updates.

Are Fibers a new thing? No

A Fiber is a generic model execution where eachunit execution works together cooperatively

Fibers are used in high-priority systems like Microsoft Windows and the OCAml programming lang for their concurrency model

Coroutines and Fibers refer to the same co-operative execution model

Building iwth Fibers
since fibers model low-level program execution, they can be used to implement new patterns that wouldn't be possible using the native call stack

#### Algebraic Effects

Error Handling in JS

**throw** - trigger an exception, doesnt dicate what should happen as a result of an error, but just signals that an error occurred

**catch** - defines the prgogam behavior for when an errro occurs. The same errro might result in different behavior in different try/catch blocks

What if could apply that pattern to logging?

**log** dictates that a log has occured

**handle log**

fetch

```
const user = fetch id;

try {

} catch () {

}
```

Algebraic Effetics

effects = a computational effect asks the calling environment to handle a particular task like logging reading from a db

effect handlers
when an effect is used the nearest effect handler is called which allows you to run code in response to the effect and return some value

language without efffects dont need iterators and async/await since those can just be built on top of effects

```
const cache = createCahce();
const UserResource = creatReousrouce(fetchUser);
const User = (props) => {
  // read signals that a suspend has occured
  const user = UserResource.read(cache, props.id);
  return <h3>{user.name}</h3>;
}
```

Suspense: A suspend effect
It's a react specific suspend effect wher ethe rect scheduler is the effect handler.

How does it work?

behind the scene the caching libary actually uses throw to signal that a suspend efffect should be triggered

# Chaos Monkeys in Your Browser: What Chaos Engineering Means for the Front End - Brian Holt

Chaos Monkey, but for your web app. Come learn how intentional failure injection is not only good for the back end but also for the front end. Hint: it means you’ll get paged less at 3AM, and I think we’re all happy about that.

Systems over time get more and more disordered

Entropy -

pinrciplesofchaois.rorg
chaois engineering is the displine of experimenting on a distributed system in order to builld confidence in the systems capibility t ow ithstand turbulent condution in production.

Tools:
spinnaker -
gramlin -
azure offers fault analysiss service

cda.ms/Cr

# Explorable Explanations with React - Joshua Comeau

Explorable explanations are dynamic, interactive, visual tools created to teach systems and concepts, the unlikely pairing of a video game and a data visualization. They’re an exciting new form of media that lets the learner develop an intuitive understanding of the subject.

In this talk, we’ll reconstruct an existing explorable explanation on waveforms and sound. We’ll start from the bottom up, using small, bite-sized components that encapsulate specific UI or behavioural concerns, working our way up to a dynamic, interactive, animated tool.

# AI For Everybody - Feather Knee (Front-End Dev @ nVidia)

Sr UI Developer GPU CLoud @ nVidia

My team is building a platform for AI in the cloud using React. The concept behind this project is to make AI accessible to anyone. "AI for you! AI for you too! And even you, over there, have some AI!" This talk will explore unique challenges of writing a complex React app in a totally new domain.

## Intro to AI

- New Yorker is a grat resource for AI influence discussion

### Plenty of influence

- piles of data - internet
- processing power
- mathematical algorithms

A: Interrrupting cow?

- toddler sees a cw, has no idea what it is
- after many cows,we always recognize them even if they're wearing a hot

model that recognizes faces
skewed matrix = same face sideways
more feedback = figure out more faces

## React for AI AI

- web interface -> api/cli -> machine learning libraries -> gpu -> job -> web interface
- shared gpu server
- ai is accessible to a wider audience
- framework agnostic platform

### Tools

#### Recompose & RxJs

- a great combo
- can use Obserable of choice
- map observable stream to React nodes

#### Reselect

- convenient way to grab values from the store
- selectors are memoized

#### Learnings

## Conclusions

# One Hundred Years of JavaScript - Justin Falcone

JavaScript is the new COBOL: widely hated but hugely successful, JS defines populist programming in the 21st century much as COBOL did in the punchcard era. But COBOL never really went away -- there are COBOL programs still running our banks and civic institutions half a century after its heyday. Many JS apps will have similar lifespans, yet we struggle with the accumulated cruft of a two year-old codebase; how will we handle a hundred years? Let's examine how software can grow and adapt over the decades, so we can write code that won't be a burden to future generations.

Agenda

- bit rot
- how programs learn
- the old and new COBOLs
- hundrd year bugs

## bit rot

## how programs learn

high cohesion, low coupling

## the old and new COBOLs

## hundred year bugs

# Conference Day 2

# Ken Wheeler - ReasonML is serious business

- Love song for your mother - his new album

* ReasonML - New syntax and toolchain for ocomal

* looks and feels like es6/s

* npm/yarn based workflows

* ML = Meta Language
* stable, mature, 20 yo language
* static types /type inference (hindley milner type system)

* compiles ocomal code to javascript
* super readable output
* great interop story

Named Arguments = ~

```
let singHook = (~name) => "hey " + name
```

## Records

```
type person = { age, int, name: string };
let Ye  = { age: 41, name: "kanye west"}
```

## Variants

```
type Album = | NotWorth ListeningTO e|Album (string);
```

## Pattern Matching

```
let amount = 5;
let result = switch(amount ) {
  | amount when amount > 5 => "whoa buddy"
  | 5 => "exact"
  | _ => "almost there"
}
```

## No nulls!

let printList = (listOfTHings) => swithc()

## No Import / Export

## try out resason

- repl.it
- sketch.sh

# Lauren Tan - Swipe Left, Uncaught TypeError: Learning to Love Type Systems

**Engineering Manager @ Netflix**

Sometimes, undefined is not a function. As mortal programmers, we ship bugs to production everyday. Bugs slow us down, frustrate our users, and cause us to have crises of confidence. Don't go alone–type systems in TypeScript, Flow, and GraphQL can improve your confidence and help you ship less bugs. We'll start with why: a practical look at what you'll get from embracing types. Then, a gentle introduction to the ideas behind them. Finally, we'll explore the possibilities of a type system over the network.

## What is a type system?

- https://github.com/donavon/undefined-is-a-function

## How many ways can this program fail?

(infinity)

```
const half = x => x / 2;

const TEST_CASES = [
  null,
  undefined,
  Symbol(1),
]

TEST_CASES.map(t => half(t))
```

## The Basics

```ts
const list1 = [1,2,3];
type ServerStacks = 'canary' | 'beta' | production'

interface User {
id: number;
name: string;
isAdmin: boolean;
}
function makeAdmin(user: User) {
user.isAdmin = 1; //error
}
```

## Generics (parametric polymorphism)

```ts
function makeArray(x: number): number[] {
  return [x];
}
function makeArray(x: number): number[] {
  return [x];
}
// let T be the type of argument X
// specify the return type functoin which is an array of objects of type T
function makeArray<T>(x: T): T[] {
  return [x];
}
```

```
function map<A,B>(fn: (item: A) => B, items: A[]) {

}
```

## Why less is better - precise types means less bugs

Your function and classes teach us

# Learning from Functional Programming

```
declare function Addition(x: number, y:number): number // proposition
```

- types are propositoins
- programs are proofs

# pure vs impure functions

- partial function is a function that is not defined for all possible input values
- total functoin is a function is defined for all possible values and will not Error or throw exceptions
- All fetch function are partial because it can fail, but you can use a monad to always get valid return
- https://github.com/gcanti/fp-ts
- https://gcanti.github.io/fp-ts/

```ts
import { Eiter, left, right } from "fp-ts";
// capture failure value
type Either<L, A> = Left<L, A> | Right<L, A>;
// does not capture failure value
```

## Pragmatic Set Theory

- `type Conferences = 'a' | 'b' | 'c'`
- If there are 3 possible options, then the
- Finite cardinality = boolean (2), null (1), undefined (1)

## Unbounded Function

```ts
function toString<T>(x: T): string {
  return x.toString();
}
// bounded "total" functoin
function toString<T>(x: NonNUllable<T>): string {
  return x.toString();
}
```

## Functional Types added with Conditional Types in 2.8

https://github.com/Microsoft/TypeScript/blob/master/lib/lib.es5.d.ts

```ts
interface ArrayLike<T> {
  readonly length: number;
  readonly [n: number]: T;
}

/**
 * Make all properties in T optional
 */
type Partial<T> = { [P in keyof T]?: T[P] };

/**
 * Make all properties in T required
 */
type Required<T> = { [P in keyof T]-?: T[P] };

/**
 * Make all properties in T readonly
 */
type Readonly<T> = { readonly [P in keyof T]: T[P] };

/**
 * From T pick a set of properties K
 */
type Pick<T, K extends keyof T> = { [P in K]: T[P] };

/**
 * Construct a type with a set of properties K of type T
 */
type Record<K extends keyof any, T> = { [P in K]: T };

/**
 * Exclude from T those types that are assignable to U
 */
type Exclude<T, U> = T extends U ? never : T;

/**
 * Extract from T those types that are assignable to U
 */
type Extract<T, U> = T extends U ? T : never;

/**
 * Exclude null and undefined from T
 */
type NonNullable<T> = T extends null | undefined ? never : T;

/**
 * Obtain the return type of a function type
 */
type ReturnType<T extends (...args: any[]) => any> = T extends (
  ...args: any[]
) => infer R
  ? R
  : any;

/**
 * Obtain the return type of a constructor function type
 */
type InstanceType<T extends new (...args: any[]) => any> = T extends new (
  ...args: any[]
) => infer R
  ? R
  : any;

/**
 * Marker for contextual 'this' type
 */
interface ThisType<T> {}
```

### Predefined conditional types

https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-8.html
TypeScript 2.8 adds several predefined conditional types to lib.d.ts:

Exclude<T, U> – Exclude from T those types that are assignable to U.
Extract<T, U> – Extract from T those types that are assignable to U.
NonNullable<T> – Exclude null and undefined from T.
ReturnType<T> – Obtain the return type of a function type.
InstanceType<T> – Obtain the instance type of a constructor function type.
Example

```ts
type T00 = Exclude<"a" | "b" | "c" | "d", "a" | "c" | "f">; // "b" | "d"
type T01 = Extract<"a" | "b" | "c" | "d", "a" | "c" | "f">; // "a" | "c"

type T02 = Exclude<string | number | (() => void), Function>; // string | number
type T03 = Extract<string | number | (() => void), Function>; // () => void

type T04 = NonNullable<string | number | undefined>; // string | number
type T05 = NonNullable<(() => string) | string[] | null | undefined>; // (() => string) | string[]

function f1(s: string) {
return { a: 1, b: s };
}

class C {
x = 0;
y = 0;
}

type T10 = ReturnType<() => string>; // string
type T11 = ReturnType<(s: string) => void>; // void
type T12 = ReturnType<(<T>() => T)>; // {}
type T13 = ReturnType<(<T extends U, U extends number[]>() => T)>; // number[]
type T14 = ReturnType<typeof f1>; // { a: number, b: string }
type T15 = ReturnType<any>; // any
type T16 = ReturnType<never>; // any
type T17 = ReturnType<string>; // Error
type T18 = ReturnType<Function>; // Error

type T20 = InstanceType<typeof C>; // C
type T21 = InstanceType<any>; // any
type T22 = InstanceType<never>; // any
type T23 = InstanceType<string>; // Error
type T24 = InstanceType<Function>; // Error

Note: The Exclude type is a proper implementation of the Diff type suggested here. We’ve used the name Exclude to avoid breaking existing code that defines a Diff, plus we feel that name better conveys the semantics of the type. We did not include the Omit<T, K> type because it is trivially written as Pick<T, Exclude<keyof T, K>>.
```

# Help me WebAssembly, you're my only hope! - Jay Phelps

Cheif Software Architect this dot - previously Netflix

WebAssembly (aka wasm) is a new, standardized compilation target for the web, shipping in all modern browsers. But since it's so low level it can be difficult to see how it will revolutionize the next generation of web apps–and definitely not just games and C++!

In this talk I will reveal what it is, how you can use it today, and the incredible opportunities it will unlock in the years to come on both the Web and on Desktop. We'll also explore how React itself might some day use WebAssembly to power it's Virtual DOM.

## What is WebAssembly? WASM

- Efficient low-level byte code for the web
- Fast to load and execute
- Streaming compilation - compiled faster than it downloads

you cna push into arry.protype totally mess up all empty array

```js
Array.protoype.push("lol");
```

- Fully spec compliant javascript would be extremely slow
- But a strict subset of javascript could be fast
- v1 mvp is est suited for Rust
- But other language are already support and more coming soon
- Things like go. net, java, ocaml

- When should i target web-assmebly
- heavily cpu bound computations
- games both unity and unreal engine offer support

- porting desktop apps

  - autocad web.autocad.com

- source-mano npm package used by creat-react-app, firefix, babel, less, etc
- ported their c++ source map code to web assembly

- https://github.com/AssemblyScript/assemblyscript
- https://webassembly.studio/
- TypeScript -> WebAssembly
  - https://webassembly.studio/?f=5u0yx178upg
- https://github.com/AssemblyScript/assemblyscript/wiki

* Tooling will eventually make the low-level a non-issue
* Textual representation to the rescue
* WebAssembly is a "stack machine" language
* Stack = data structure with "push" or "pop"

## Why not use it now?

- There 's no direct access to host APIs (eg the DOM), you have to call into JavScript right now. The bridge over JS is very expensive.
- Garbage Collection - necessary for better inerop with JavaScript and WebIDL
- Multi-threading - SharedArrayBuffer re-enabled in Chrome 69 Beta
- React component could also run in WebAssembly (writing them in Reason)

## New things happening

- Ember's Glimmer VM is being written in Rust compiled to WebAssembly
- github.com/mbasso/awesome-wasm

# Hot Garbage: Clean Code is Dead - Michael Chan

The Code is rising up to enslave us. An army of linter-plugins have given it a voice and it's angry. Clean code isn't the goal, its the enemy. Great code isn't clean, it's hot garbage—hot-swappable and easy to throw out. Code is a means to an end. When we stop fetishizing code and start fighting it, we've found the right enemy and we can get back to to the good work of serving customers.

# I'm exploring a monorepo arhcitecture for my legos so I cant find you a single lego set
