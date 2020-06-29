<script>
  import { createEventDispatcher } from 'svelte'
  const dispatch = createEventDispatcher()

  export let title, summary, labelA, labelB
  export let op = "+", a = "", b = "", formula = (a, b) => '???', showRefresh = null

  $: value = formula(a, b)
</script>

<div class="card">
  <h2>{title}</h2>
  <p>{summary}</p>
  <div class="container">

  <div class="field">
    <label>{labelA}</label>
    <div class="input-wrapper">
      <input bind:value={a}/>

      {#if showRefresh=="a"}
      <button on:click|preventDefault={() => dispatch('refresh')} title="Generate another">
        <svg width="16px" height="16px" viewBox="0 0 20 20" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
            <g id="Page-1" stroke="none" stroke-width="1" fill="none" fill-rule="evenodd">
                <g id="refresh" fill="#000000">
                    <path d="M10,3 C6.13400675,3 3,6.13400675 3,10 C3,11.9329966 3.78350169,13.6829966 5.05025253,14.9497475 L6.46446609,13.5355339 C5.55964406,12.6307119 5,11.3807119 5,10 C5,7.23857625 7.23857625,5 10,5 L10,3 L10,3 Z M14.9497475,5.05025253 C16.2164983,6.31700338 17,8.06700338 17,10 C17,13.8659932 13.8659932,17 10,17 L10,15 C12.7614237,15 15,12.7614237 15,10 C15,8.61928813 14.4403559,7.36928813 13.5355339,6.46446609 L14.9497475,5.05025253 L14.9497475,5.05025253 Z M10,20 L6,16 L10,12 L10,20 L10,20 Z M10,8 L14,4 L10,0 L10,8 L10,8 Z" id="Combined-Shape"></path>
                </g>
            </g>
        </svg>
      </button>
      {/if}
    </div>
  </div>

  <span class="op">
    {op}
  </span>

  <div class="field">
    <label>{labelB}</label>
    <div class="input-wrapper">
      <input bind:value={b}/>

      {#if showRefresh=="b"}
      <button on:click|preventDefault={() => dispatch('refresh')} title="Generate another">
        <svg width="16px" height="16px" viewBox="0 0 20 20" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
            <g id="Page-1" stroke="none" stroke-width="1" fill="none" fill-rule="evenodd">
                <g id="refresh" fill="#000000">
                    <path d="M10,3 C6.13400675,3 3,6.13400675 3,10 C3,11.9329966 3.78350169,13.6829966 5.05025253,14.9497475 L6.46446609,13.5355339 C5.55964406,12.6307119 5,11.3807119 5,10 C5,7.23857625 7.23857625,5 10,5 L10,3 L10,3 Z M14.9497475,5.05025253 C16.2164983,6.31700338 17,8.06700338 17,10 C17,13.8659932 13.8659932,17 10,17 L10,15 C12.7614237,15 15,12.7614237 15,10 C15,8.61928813 14.4403559,7.36928813 13.5355339,6.46446609 L14.9497475,5.05025253 L14.9497475,5.05025253 Z M10,20 L6,16 L10,12 L10,20 L10,20 Z M10,8 L14,4 L10,0 L10,8 L10,8 Z" id="Combined-Shape"></path>
                </g>
            </g>
        </svg>
      </button>
      {/if}

    </div>
  </div>

  <span class="op">
    =
  </span>

  <span class="output">
    <a href="https://domains.google.com/m/registrar/search?searchTerm={value}&tab=1" target="_blank">{ value }</a>
    <slot/>
  </span>
  </div>
</div>

<style>
  .card {
    background: floralwhite;
    padding: 2rem;
    margin-bottom: 2rem;
    border-radius: 0.2em;
    border: solid 1px white;
    font-size: 1.3rem;
  }

  .card p {
    color: #888;
  }

  .container {
    display: flex;
  }

  .container > span {
    padding: 0.2rem;
  }

  .op, .output {
    font-weight: bold;
    color: #666;
    align-self: flex-end;
    line-height: 2rem;
  }

  .op {
    margin: 0 0.5rem;
  }

  .output a {
    color: cornflowerblue;
    font-size: 1.2em;
  }

  h2 {
    color: #444;
  }

  .field label {
    font-size: 0.9rem;
    margin: 0 0 0.5em 0;
  }

  .input-wrapper {
    border: solid 1px #ccc;
    display: flex;
    border-radius: 0.3rem;
    background: #fff;
  }

  .input-wrapper input {
    width: 100%;
    border: none;
    background: none;
    margin: 0;
  }

  .input-wrapper input:focus {
    outline: none;
  }
  .input-wrapper button {
    flex: 1;
    margin: 0;
  }

  button {
    color: limegreen;
    background: none;
    border: none;
    cursor: pointer;
  }
  button:hover {
    color: darkgreen;
  }
  svg path {
    fill: currentColor
  }
</style>
