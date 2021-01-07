# Simple Svelte Select

Inspired by the Svelte Select

## Installation

````bash
yarn add simple-svelte-select
````

or 

````bash
npm install simple-svelte-select
````

## Repl

[Actual version](https://svelte.dev/repl/31cba90b9da74c2ebd5d8b10f5d1ee6c?version=3.31.0 "Svelte Repl")

## Simple example

````jsx
<script>
import Select from 'simple-svelte-select';

const items = [
  { value: 'apple', label: "Tasty apple" },
  { value: 'lemon', label: "Juicy lemon" }
];

let selectedValue = items[0];
</script>

<Select items={items} bind:selectedValue={selectedValue} placeholder="Select apple or lemon"/>
````
## Setting up

By default Select prohibits entering a search query on mobile devices (If the device width is less than 640px). You can pass the disabled property to control this yourself

````jsx
//This Select does not allow you to search
<Select {items} bind:selectedValue disabled/>
//This default Select allows you to search if your device (or browser window) is wider than 640 pixels
<Select {items} bind:selectedValue/>
//This select allows you to search
<Select {items} bind:selectedValue disabled={false}/>
````

## Async

See example in [Repl](https://svelte.dev/repl/975f82c7fd1a4f4c81ec16155ad16770?version=3.31.0)

## Styling

Go to the website (https://sss-configurator.web.app/). 
Adjust the styles there
Then copy the theme
Look at the picture below how to use the site

![screenshot](https://i.imgur.com/2iWAGQf.png)

````jsx

//Get this from this(sss-configurator.web.app) site then paste it into your code like this
const styles = {"light":{"bgCol":"red","bCol":"yellow","cACol":"orange","bHCol":"blue","selCol":"#000","iTCol":"#3f4f5f","iS":"0 2px 3px 0 rgba(44, 62, 80, 0.24)","iHCol":"#E7F2FF","iACol":"#e2efff","iTNSCol":"#000","iATCol":"#fff","iSACol":"#007bff"},"dark":{"bgCol":"#161616","bCol":"#333","cACol":"#006fe8","bHCol":"#444","selCol":"#eee","iTCol":"#eee","iS":"0 2px 2px 0 rgba(24, 24, 24, 0.24)","iHCol":"#252525","iACol":"#323232","iTNSCol":"#eee","iATCol":"#fff","iSACol":"#414141"}}

let selectedValue;

const items = new Array(50)
		.fill('')
		.map((_, i) => ({ value: `item-${i + 1}`, label: `Item ${i + 1}` }))

//Then pass it into your select
<Select {items} bind:selectedValue styles={styles}/>
````