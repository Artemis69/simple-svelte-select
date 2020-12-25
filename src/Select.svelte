<script>
    import { clickOutside } from './clickOutside.js';
  
    export let items = undefined;
    export let placeholder = 'Select...';
    export let selectedValue = undefined;
  
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
      .selectContainer{
          background: #fff;
          border: 1px solid #d8dbdf;
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
      .selectContainer:hover{
          border-color: #b2b8bf;
      }
      .selectContainer.active{
          border-color: #006fe8;
          outline: none;
      }
      .selectedValue{
              border-radius: 3px;
          background: #fff;
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
      .selectedValue > *{
          margin: 0 16px;
      }
      .inputContainer{
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
          background: #fff;
          z-index: 2;
      }
      .inputContainer input{
              border-radius: 3px;
          font-size: 1em;
          color: #3f4f5f;
          padding: 0;
          line-height: 42px;
          height: 100%;
          letter-spacing: -0.08px;
          cursor: default;
          width: calc(100% - 32px);
          border: none;
          margin: 0;
      }
      .inputContainer input:focus{
          outline: none;
      }
      .items{
          position: absolute;
          left: 0;
          right: 0;
          top: 48px;
          background-color: #fff;
          border-radius: 4px;
          -webkit-box-shadow: 0 2px 3px 0 rgba(44, 62, 80, 0.24);
                  box-shadow: 0 2px 3px 0 rgba(44, 62, 80, 0.24);
          overflow-y: auto;
          z-index: 4;
          max-height: 250px;
      }
      .itemContainer{
          width: 100%;
          height: 42px;
          background: #fff;
          display: -webkit-box;
          display: flex;
          -webkit-box-pack: center;
                  justify-content: center;
          -webkit-box-align: center;
                  align-items: center;
      }
      .itemContainer:hover{
          background: #E7F2FF;
      }
      .itemContainer:active{
          background: #e2efff;
      }
      .item{
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
      .item span{
          font-size: 1em;
      }
      .remove{
          cursor: pointer;
          margin-top: -4px;
          font-size: 1.5em;
      }
      .itemContainer.active{
          background: #007bff;
          color: #fff;
      }
      input:disabled {
          background: #fff;
      }
  @media (prefers-color-scheme: dark) {
      .selectContainer{
          background: #374151;
          border: 1px solid #4B5563;
      }
      .selectContainer:hover{
          border-color: #535e6e;
      }
      .selectContainer.active:hover{
          border-color: #006fe8;
      }
      .selectedValue{
          background: #374151;
      }
      .inputContainer{
          background: #374151;
      }
      .itemContainer{
          background: #374151;
      }
      .itemContainer:hover{
          background: #4B5563;
      }
      .itemContainer:active{
          background: #535f6e;
      }
      .items{
          background: #374151;
      }
      .inputContainer input{
          background: #374151;
          color: #eee;
      }
      input:disabled {
          background: #374151;
      }
      .inputContainer input::-webkit-input-placeholder {
          color: #ddd;
      }
      .inputContainer input::-moz-placeholder {
          color: #ddd;
      }
      .inputContainer input:-ms-input-placeholder {
          color: #ddd;
      }
      .inputContainer input::-ms-input-placeholder {
          color: #ddd;
      }
      .inputContainer input::placeholder {
          color: #ddd;
      }
      .item span{
          color: #eee;
      }
      .selectContainer *{
          color: #eee;
      }
      .itemContainer.active{
          background: #6B7280;
      }
      .itemContainer.active .item span{
          color: #eee;
      }
  }
  ::-webkit-scrollbar {
      width: 8px;
      height: 8px;
      background-color: rgba(0,0,0,0);
      -webkit-border-radius: 100px;
  }
  
  ::-webkit-scrollbar:hover {
      background-color: rgba(0, 0, 0, 0.09);
  }
  
  ::-webkit-scrollbar-thumb {
      background: rgba(0,0,0,0.5);
      -webkit-border-radius: 100px;
  }
  ::-webkit-scrollbar-thumb:active {
      background: rgba(0,0,0,0.61); 
      -webkit-border-radius: 100px;
  }
  
  ::-webkit-scrollbar-thumb:vertical {
    min-height: 10px;
  }
  ::-webkit-scrollbar-thumb:horizontal {
    min-width: 10px;
  }
  ::-webkit-resizer{
      background-color:transparent;
  }
  </style>