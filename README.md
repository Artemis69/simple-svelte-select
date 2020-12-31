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

1.0.13 Version

[Simple](https://svelte.dev/repl/d8af37de112a45f3a3da2ffee0e7eea7?version=3.31.0 "Svelte Repl")

Actual version

[Simple](https://svelte.dev/repl/a7ae80f2cac54bed810f576fe2655b06?version=3.31.0 "Svelte Repl")

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
<Select items bind:selectedValue disabled/>
//This default Select allows you to search if your device (or browser window) is wider than 640 pixels
<Select items bind:selectedValue/>
//This select allows you to search
<Select items bind:selectedValue disabled={false}/>
````

## Async

See example in [Repl](https://svelte.dev/repl/975f82c7fd1a4f4c81ec16155ad16770?version=3.31.0)

## Styling

You can change the coloring of Select using CSS variables.
By default, Select includes a light and a dark theme.
Read more about css variables [here](https://developer.mozilla.org/ru/docs/Web/CSS/Using_CSS_custom_properties "MDN Web Docs").
You just have to specify the variable and its value. Use "!important" to make the colors apply.
Take a look at the example below:
````css
:root{
/* Background color */
--simple-svelte-select-background-color: #fff !important;
/* Select active border color */
--simple-svelte-select-container-active-color: #006fe8 !important;
/* Select border color */
--simple-svelte-select-border-color: #d8dbdf !important;
/* Select border color hover */
--simple-svelte-select-border-hover-color: #b2b8bf !important;
/* Item background color; Selected item in a list */
--simple-svelte-select-item-state-active-color: #007bff !important;
/* Item text color; Selected item in a list */
--simple-svelte-select-item-active-text-color: #fff !important;
/* Items shadow */
 --simple-svelte-select-items-shadow: 0 2px 3px 0 rgba(44, 62, 80, 0.24) !important;
/* Each item :hover color */
--simple-svelte-select-item-hover-color: #E7F2FF !important;
/* Each item :active color */
--simple-svelte-select-item-active-color: #e2efff !important;
/* Input text color */
--simple-svelte-select-input-text-color: #3f4f5f !important;
/* Input placeholder color */
--simple-svelte-select-input-placeholder-color: #333 !important;
/* Selected value color */
--simple-svelte-select-selected-color: #000 !important;
/* Item text color (not selected) */
--simple-svelte-select-item-text-color: #000 !important;
}
````
Go to [example](https://svelte.dev/repl/1bca66aa2644426e83a1381a117d9fe4?version=3 "Svelte Repl") for more insights

Using the media query you can set up a dark theme
````css
@media (prefers-color-scheme: dark) {
	 :root {
		/* HERE */
	}
}
````

If you dont like new theming you can install 1.0.13 version

````bash
yarn add simple-svelte-select@1.0.13
````