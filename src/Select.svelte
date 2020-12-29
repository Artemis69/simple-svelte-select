<script>
    import { clickOutside } from './clickOutside.js';
  
    export let items = undefined;
    export let placeholder = 'Select...';
    export let selectedValue = undefined;

	export let disabled = window.matchMedia('(max-width: 640px)').matches;
  
    let isActive = false;
  
    if (items.length == 0) throw new Error('items should not be empty');
      
    let value;
      
    const inputHandler = () => {
        if(value != ''){
            for(let i = 0; i < items.length; i++){
                if(items[i].label.toLowerCase().search(value.trim().toLowerCase()) == -1){
                    items[i].hidden = true;
                }else{
                    items[i].hidden = false;
                }
            }
        }else{
            for(let i = 0; i < items.length; i++){
                items[i].hidden = false;
            }
        }
    }
      
    const clearSearch = () => {
        value = undefined;
        for(let i = 0; i < items.length; i++){
            items[i].hidden = false;
        }
    }

</script>
  
{#if items.length != 0}
    <div class="selectContainer" class:active={isActive} use:clickOutside on:click_outside={() => {isActive = false}}>
        {#if selectedValue}
            <div class="selectedValue" on:click={() => isActive = !isActive}>
                <span>
                    {selectedValue.label}
                </span>
                <span class="remove" on:click={() => {selectedValue = undefined; isActive = false; clearSearch();}}>
                    &times;
                </span>
            </div>
        {/if}
        <div class="inputContainer" on:click={() => isActive = !isActive}>
            <input 
                type="text" 
                placeholder={placeholder} 
                autocomplete="off" 
                autocorrect="off" 
				spellcheck="false"
				disabled={disabled}
                bind:value={value}
                on:input={inputHandler}
            >
        </div>
        {#if isActive}
            <div class="items">
                {#each items as item, i}
                    {#if !item.hidden}
                        <div class="itemContainer" class:active="{selectedValue == items[i]}">
                            <div class="item" on:click={() => {isActive = false; selectedValue=items[i]; clearSearch();}}>
                                <span>{item.label}</span>
                            </div>
                        </div>
                    {/if}
                {/each}
            </div>
        {/if}
      </div>
{/if}
  
<style>
:root {
	--simple-svelte-select-container-active-color: #006fe8;
	--simple-svelte-select-item-state-active-color: #007bff;
	--simple-svelte-select-item-active-text-color: #fff;
	--simple-svelte-select-background-color: #fff;
	--simple-svelte-select-border-color: #d8dbdf;
	--simple-svelte-select-border-hover-color: #b2b8bf;
	--simple-svelte-select-items-shadow: 0 2px 3px 0 rgba(44, 62, 80, 0.24);
	--simple-svelte-select-item-hover-color: #E7F2FF;
	--simple-svelte-select-item-active-color: #e2efff;
	--simple-svelte-select-input-text-color: #3f4f5f;
	--simple-svelte-select-input-placeholder-color: #333;
	--simple-svelte-select-selected-color: #000;
	--simple-svelte-select-item-text-color: #000;
}

@media (prefers-color-scheme: dark) {
	 :root {
		--simple-svelte-select-container-active-color: #006fe8;
		--simple-svelte-select-item-state-active-color: #414141;
		--simple-svelte-select-item-active-text-color: #fff;
		--simple-svelte-select-background-color: #161616;
		--simple-svelte-select-border-color: #333;
		--simple-svelte-select-border-hover-color: #444;
		--simple-svelte-select-item-hover-color: #252525;
		--simple-svelte-select-item-active-color: #323232;
		--simple-svelte-select-input-text-color: #eee;
		--simple-svelte-select-input-placeholder-color: #ddd;
		--simple-svelte-select-selected-color: #eee;
		--simple-svelte-select-item-text-color: #eee;
	}
}

.selectContainer {
	background: var(--simple-svelte-select-background-color);
	border: 1px solid var(--simple-svelte-select-border-color);
	border-radius: 3px;
	padding: 0 16px;
	position: relative;
	display: -webkit-box;
	display: flex;
	-webkit-box-align: center;
	align-items: center;
	height: 42px;
	-webkit-user-select: none;
	-moz-user-select: none;
	user-select: none;
	width: 100%;
	min-width: 152px;
}

.selectContainer:hover {
	border-color: var(--simple-svelte-select-border-hover-color);
}

.selectContainer.active {
	border-color: var(--simple-svelte-select-container-active-color);
	outline: none;
}

.selectedValue {
	border-radius: 3px;
	background: var(--simple-svelte-select-background-color);
	position: absolute;
	height: 100%;
	left: 0;
	right: 0;
	display: -webkit-box;
	display: flex;
	-webkit-box-pack: justify;
	justify-content: space-between;
	-webkit-box-align: center;
	align-items: center;
	z-index: 3;
	line-height: 42px;
}
.selectedValue span{
	color: var(--simple-svelte-select-selected-color);
}

.selectedValue>* {
	margin: 0 16px;
}

.inputContainer {
	border-radius: 3px;
	position: absolute;
	height: 100%;
	left: 0;
	right: 0;
	display: -webkit-box;
	display: flex;
	-webkit-box-pack: center;
	justify-content: center;
	-webkit-box-align: center;
	align-items: center;
	background: var(--simple-svelte-select-background-color);
	z-index: 2;
}

.inputContainer input {
	background: transparent;
	border-radius: 3px;
	font-size: 1em;
	color: var(--simple-svelte-select-input-text-color);
	padding: 0;
	line-height: 42px;
	height: 100%;
	letter-spacing: -0.08px;
	cursor: default;
	width: calc(100% - 32px);
	border: none;
	margin: 0;
}

.inputContainer input:focus {
	outline: none;
}

.items {
	position: absolute;
	left: 0;
	right: 0;
	top: 48px;
	background-color: var(--simple-svelte-select-background-color);
	border-radius: 4px;
	-webkit-box-shadow: var(--simple-svelte-select-items-shadow);
	box-shadow: var(--simple-svelte-select-items-shadow);
	overflow-y: auto;
	z-index: 4;
	max-height: 250px;
}

.itemContainer {
	width: 100%;
	height: 42px;
	background: var(--simple-svelte-select-background-color);
	display: -webkit-box;
	display: flex;
	-webkit-box-pack: center;
	justify-content: center;
	-webkit-box-align: center;
	align-items: center;
}

.itemContainer:hover {
	background: var(--simple-svelte-select-item-hover-color);
}

.itemContainer:active {
	background: var(--simple-svelte-select-item-active-color);
}

.item {
	width: calc(100% - 32px);
	height: 42px;
	display: -webkit-box;
	display: -ms-flexbox;
	display: flex;
	-webkit-box-pack: start;
	justify-content: flex-start;
	-webkit-box-align: center;
	align-items: center;
}

.item span {
	font-size: 1em;
}

.itemContainer:not(.active) .item span{
	color: var(--simple-svelte-select-item-text-color);
}

.remove {
	cursor: pointer;
	font-size: 1.5em;
    margin-top: -4px !important;
}

.itemContainer.active {
	background: var(--simple-svelte-select-item-state-active-color);
	color: var(--simple-svelte-select-item-active-text-color);
}

.inputContainer input::-webkit-input-placeholder {
	color: var(--simple-svelte-select-input-placeholder-color);
}

.inputContainer input::-moz-placeholder {
	color: var(--simple-svelte-select-input-placeholder-color);
}

.inputContainer input::placeholder {
	color: var(--simple-svelte-select-input-placeholder-color);
}


/* Scroll */

 ::-webkit-scrollbar {
	width: 8px;
	height: 8px;
	background-color: rgba(0, 0, 0, 0);
	-webkit-border-radius: 100px;
}

 ::-webkit-scrollbar:hover {
	background-color: rgba(0, 0, 0, 0.09);
}

 ::-webkit-scrollbar-thumb {
	background: rgba(0, 0, 0, 0.5);
	-webkit-border-radius: 100px;
}

 ::-webkit-scrollbar-thumb:active {
	background: rgba(0, 0, 0, 0.61);
	-webkit-border-radius: 100px;
}

 ::-webkit-scrollbar-thumb:vertical {
	min-height: 10px;
}

 ::-webkit-scrollbar-thumb:horizontal {
	min-width: 10px;
}

 ::-webkit-resizer {
	background-color: transparent;
}
</style>