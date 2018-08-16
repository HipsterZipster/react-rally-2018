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
