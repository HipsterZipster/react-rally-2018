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
