# ink-quicksearch-input

> QuickSearch Component for [Ink 2](https://github.com/vadimdemedes/ink), [demo on repl.it](https://repl.it/@johnosullivan1/ink-quicksearch-input)

Forked from [@eximchain](https://github.com/Eximchain)'s fork [`ink-quicksearch`](https://github.com/Eximchain/ink-quicksearch-input) of [@aicioara](https://github.com/aicioara) [`ink-quicksearch`](https://github.com/aicioara/ink-quicksearch) and component to upgrade it to Ink 3 on top of the original Ink 2 support.  Big thanks to both of them for laying out the original logic in v1 and v2! If you are looking for a component that works with Ink v1, that's where to go.  This re-write uses modern React (e.g. function components and hooks), and it is also in Typescript, improving the developer experience.  The only behavioral difference is that this component always filters out items which do not match the query.  Note that the demo runs a good bit slower than it does in an actual terminal; there's some uncanny-valley lag which is not present during normal use.

## Install

```
$ npm install ink-quicksearch-input-3
```

## Quickstart

If you'd like to get a feel for how the component works, you can see the examples in action by running:

```bash
npm install
npm start
```

## Usage

```jsx
import React, { useState } from 'react';
import { render, Box, Text } from 'ink';
import QuickSearchInput from 'ink-quicksearch-input-3';

const Demo = (props) => {
  const [result, setResult] = useState('');
  return (
    <Box>
      <Text>The user selected {result}.</Text>
      <QuickSearchInput 
        items={[
          {value: 1, label: 'Animal'},
          {value: 3, label: 'Antilope'},
          {value: 2, label: 'Animation'},
          {value: 0, label: 'Animate'},
          {value: 4, label: 'Arizona'},
          {value: 5, label: 'Aria'},
          {value: 6, label: 'Arid'},
        ]}
        onSelect={(item) => setResult(item.label)}
      />
    </Box>
  )
};

render(<Demo/>);
```


## Props

| Parameter | Type | Default | Description
| --- | --- | --- | --- |
| items | `Array(object)` | `[]` | Items to display in a list. <br> Each item must be an object and have at least a `label` prop. 
| onSelect | `function` | | Function to call when user selects an item. <br> Item object is passed to that function as an argument.
| focus | `boolean` | `true` | Listen to user's input. <br> Useful in case there are multiple input components at the same time and input must be "routed" to a specific component.
| caseSensitive | `boolean` | `false` | Whether or not quicksearch matching will be case sensitive.
| limit | `int` | `0` | Limit the number of rows to display. `0` is unlimited <br> Use in conjunction with https://www.npmjs.com/package/term-size.
| forceMatchingQuery | `bool` | `false` | If set to true, queries that return no results are not allowed. In particular, if previous query `X` returns at least one result and `X + new_character` would not, query will not update to `X + new_character`.
| clearQueryChars | `Array(char)` | `['\u0015', '\u0017']` <br> (<kbd>Ctrl</kbd> + <kbd>u</kbd>, <kbd>Ctrl</kbd> + <kbd>w</kbd>) | Key Combinations that will clear the query. <br> `ch` follows the `keypress` API `process.stdin.on('keypress', (ch, key) => {})`.
| initialSelectionIndex | `int` | `0` | Selection index when the component is initially rendered or when `props.items` changes. Can be set together with new `props.items` to automatically select an option.
| label | `string` | | Optionally provide a label which will appear before the current query.
| indicatorComponent | Component | | Custom component to override the default indicator component (default - arrow).
| itemComponent | Component | | Custom component to override the default item style (default - selection coloring).
| highlightComponent | Component | | Custom component to override the default highlight style (default - background highlight).
| statusComponent | Component | | Custom component to override the status component (default - current query, optional value label).

## Component Props

### indicatorComponent

Type: `Component`<br>
Props:

- `isSelected`: `boolean`
- `isHighlighted`: `boolean`
- `item`: `Object` - The corresponding object inside `props.items`


### itemComponent

Type: `Component`<br>
Props:

- `isSelected`: `boolean`
- `isHighlighted`: `boolean`
- `item`: `Object` - The corresponding object inside `props.items`
- `children`: `any`


### highlightComponent

Type: `Component`<br>
Props:

- `isSelected`: `boolean`
- `isHighlighted`: `boolean`
- `item`: `Object` - The corresponding object inside `props.items`
- `children`: `any`


### statusComponent

Type: `Component`<br>
Props:

- `hasMatch`: `boolean`
- `children`: `any`
- `label`: Optional `string`



## Default Components

```jsx
// For the following four, whitespace is important
const IndicatorComponent = ({isSelected}) => {
    return <Color hex="#00FF00">{isSelected ? '>' : ' '} </Color>;
};

const ItemComponent = ({isSelected, children}) => (
    <Color hex={isSelected ? '#00FF00' : ''}>{children}</Color>
);

const HighlightComponent = ({children}) => (
    <Color bgHex="#6C71C4">{children}</Color>
);

const StatusComponent = ({hasMatch, children}) => (
    <Color hex={hasMatch ? '#00FF00' : '#FF0000'}>{children}</Color>
);
```
