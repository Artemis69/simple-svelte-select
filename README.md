# Simple Svelte Select

## Installation

````bash
yarn add simple-svelte-select
````

or 

````bash
npm install simple-svelte-select
````

## Repl

https://svelte.dev/repl/d8af37de112a45f3a3da2ffee0e7eea7?version=3.31.0

## Simple example

````jsx
<script>
import Select from 'simple-svelte-select';

items = [
  { value: 'apple', label: "Tasty apple" },
  { value: 'lemon', label: "Juicy lemon" }
];

let selectedValue = items[0];
</script>

<Select items={items} bind:selectedValue={selectedValue} placeholder="Select apple or lemon"/>
````

Thats all