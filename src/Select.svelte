<script>
    import { clickOutside } from './clickOutside.js';
  
    export let items = undefined;
    export let placeholder = 'Select...';
    export let selectedValue = undefined;

    export let disabled = window.matchMedia('(max-width: 640px)').matches;
    
    let isActive = false;
   
	let value;
	
	if((!items || items.length === 0) && selectedValue) throw Error('To be able to select value items must be defined');
      
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

    import { css } from 'goober';

    export let styles = {
        light: {
            bgCol: '#fff',
            bCol: '#d8dbdf',
			cACol: '#006fe8',
			bHCol: '#b2b8bf',
			selCol: '#000',
			iTCol: '#3f4f5f',
			iS: '0 2px 3px 0 rgba(44, 62, 80, 0.24)',
			iHCol: '#E7F2FF',
			iACol: '#e2efff',
			iTNSCol: '#000',
			iATCol: '#fff',
			iSACol: '#007bff',
        },
        dark: {
            bgCol: '#161616',
            bCol: '#333',
			cACol: '#006fe8',
			bHCol: '#444',
			selCol: '#eee',
			iTCol: '#eee',
			iS: '0 2px 3px 0 rgba(24, 24, 24, 0.24)',
			iHCol: '#252525',
			iACol: '#323232',
			iTNSCol: '#eee',
			iATCol: '#fff',
			iSACol: '#414141',
        }
    }

	export let dark = window.matchMedia('(prefers-color-scheme: dark)').matches;
  

	const selectContainerStyle = css(`background: ${dark ? styles.dark.bgCol : styles.light.bgCol};border: 1px solid ${dark ? styles.dark.bCol : styles.light.bCol};outline: none;border-radius:3px;padding:0 16px;position:relative;display:-webkit-box;display:flex;-webkit-box-align:center;align-items:center;height:42px;-webkit-user-select:none;-moz-user-select:none;user-select:none;width:100%;min-width:152px;&.active{border: 1px solid ${dark ? styles.dark.cACol : styles.light.cACol};}&:hover{border: 1px solid ${dark ? styles.dark.bHCol : styles.light.bHCol};}`);
	const selectedValueStyle = css(`position: absolute;height: 100%;left: 0;right: 0;display: -webkit-box;display: flex;-webkit-box-pack: justify;justify-content: space-between;-webkit-box-align: center;align-items: center;z-index: 3;line-height: 42px;border-radius: 3px;background: ${dark ? styles.dark.bgCol : styles.light.bgCol};&>span{color: ${dark ? styles.dark.selCol : styles.light.selCol};margin: 0 16px;}`);
	const removeStyle = css(`cursor:pointer;font-size: 1.5em;margin-top:-4px!important;`);
	const inputContainerStyle = css(`border-radius: 3px;position: absolute;height: 100%;left: 0;right: 0;display: -webkit-box;display: flex;-webkit-box-pack: center;justify-content: center;-webkit-box-align: center;align-items: center;background: ${dark ? styles.dark.bgCol : styles.light.bgCol};z-index: 2;`);
	const inputStyle = css(`background: transparent;border-radius: 3px;font-size: 1em;color: ${dark ? styles.dark.iTCol : styles.light.iTCol};padding:0;line-height:42px;height:100%;letter-spacing:-0.08px;cursor:default;width:calc(100% - 32px);border:none;margin:0;&:focus{outline:none;}`);
	const itemsStyle = css(`position: absolute;left: 0;right: 0;top: 48px;background-color:${dark ? styles.dark.bgCol : styles.light.bgCol};border-radius: 4px;-webkit-box-shadow: ${dark ? styles.dark.iS : styles.light.iS};box-shadow: ${dark ? styles.dark.iS : styles.light.iS};overflow-y: auto;z-index: 4;max-height: 250px;&::-webkit-scrollbar {width: 8px;height: 8px;background-color: rgba(0, 0, 0, 0);-webkit-border-radius: 100px;}&::-webkit-scrollbar:hover {background-color: rgba(0, 0, 0, 0.09);}&::-webkit-scrollbar-thumb {background: rgba(0, 0, 0, 0.5);-webkit-border-radius: 100px;}&::-webkit-scrollbar-thumb:active {background: rgba(0, 0, 0, 0.61);-webkit-border-radius: 100px;}&::-webkit-scrollbar-thumb:vertical {min-height: 10px;}&::-webkit-scrollbar-thumb:horizontal {min-width: 10px;}&::-webkit-resizer{background-color: transparent;}`);
	const itemContainerStyle = css(`width: 100%;height: 42px;background: ${dark ? styles.dark.bgCol : styles.light.bgCol};display: -webkit-box;display: flex;-webkit-box-pack: center;justify-content: center;-webkit-box-align:center;align-items:center;color:${dark ? styles.dark.iTNSCol : styles.light.iTNSCol};	&:hover{background: ${dark ? styles.dark.iHCol : styles.light.iHCol};}&:active{background:${dark ? styles.dark.iACol : styles.light.iACol};}&.active{color:${dark ? styles.dark.iATCol : styles.light.iATCol};background:${dark ? styles.dark.iSACol : styles.light.iSACol};}`);
	const itemStyle = css(`width: calc(100% - 32px);height: 42px;display: -webkit-box;display: flex;-webkit-box-pack: start;justify-content: flex-start;-webkit-box-align:center;align-items:center;&>span{font-size: 1em;}`);

</script>

<div class={selectContainerStyle} class:active={isActive} use:clickOutside on:click_outside={() => {isActive = false}}>
        {#if selectedValue}
            <div class={selectedValueStyle} on:click={() => isActive = !isActive}>
                <span>
                    {selectedValue.label}
                </span>
                <span class={removeStyle} on:click={() => {selectedValue = undefined; isActive = false; clearSearch();}}>
                    &times;
                </span>
			</div>
		{:else}
			<div class={inputContainerStyle} on:click={() => isActive = !isActive}>
				{#if items && items.length > 0}
					{#if !disabled}
						<input 
							type="text" 
							placeholder={placeholder} 
							autocomplete="off" 
							autocorrect="off" 
							spellcheck="false"
							disabled={disabled}
							bind:value={value}
							on:input={inputHandler}
							class={inputStyle}
						>
					{:else}
						<p class={inputStyle}>
							{placeholder}
						</p>
					{/if}
				{:else}
					<input 
						type="text" 
						placeholder={placeholder} 
						disabled
						class={inputStyle}
					>
				{/if}
			</div>
        {/if}
        {#if isActive}
            <div class={itemsStyle}>
				{#if items && items.length > 0}
					{#each items as item, i}
						{#if !item.hidden}
							<div class={itemContainerStyle} class:active="{selectedValue == items[i]}" on:click={() => {isActive = false; selectedValue=items[i]; clearSearch();}}>
								<div class={itemStyle}>
									<span>{item.label}</span>
								</div>
							</div>
						{/if}
					{/each}
				{:else}
					<div class={itemContainerStyle}>
						<div class={itemStyle} on:click={() => {isActive = false;}}>
							<span>No items found</span>
						</div>
					</div>
				{/if}
            </div>
        {/if}
</div>