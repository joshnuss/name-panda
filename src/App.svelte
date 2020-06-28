<script>
  import Equation from './Equation.svelte'
  import animals from './animals.js'
  import adjectives from './adjectives.js'
  import { onMount } from 'svelte'

  const endings = [
    "ify",
    "ly",
    "io"
  ]
  const vowels = "aeiouy"

  let name = "cart"
  let animal = animals[getRandomInt(animals.length)]
  let ending = endings[getRandomInt(endings.length)]
  let adjective = 'smart'
  let first = 'accent', second = 'future'
  let checked = []

  onMount(() => {
    checked[5].checked = false
    checked[6].checked = false
    checked[7].checked = false
  })

  $: {
    checked = (first + second).split('').map(char => {
      return {letter: char, checked: true}
    })
  }
  $: value = checked.reduce((acc, char) =>  { return acc + (char.checked ? char.letter : '') }, '')

  function refreshAnimal() {
    const index = getRandomInt(animals.length)
    animal = animals[index]
  }

  function refreshEnding() {
    const index = getRandomInt(endings.length)
    ending = endings[index]
  }

  function refreshAdjective() {
    const index = getRandomInt(adjectives.length)
    adjective = adjectives[index]
  }

  function getRandomInt(max) {
    return Math.floor(Math.random() * Math.floor(max));
  }

  function replaceEndings(a, b) {
    let replaced = a.replace(new RegExp(`${b}$`), "")
    if (replaced != a) return replaced

    replaced = a.replace(new RegExp(`^${b}`), "")
    if (replaced != a) return replaced

    return a.replace(new RegExp(b, 'g'), "")
  }

  function toggleChecked(index) {
    checked[index].checked = !checked[index].checked
    checked = checked
  }
</script>

<!-- TODO add local storage store -->
<!-- TODO when refreshing make sure to not use last value -->


<h1>
  SaaS Name Generator
</h1>

<Equation title="Animals" summary="Add an animal to the end of a name." bind:a={name} bind:b={animal} formula={(a, b) => `${a} ${b}`} showRefresh="b" on:refresh={refreshAnimal}/>

<Equation title="Appending" summary="Append a short ending to your name." bind:a={name} bind:b={ending} formula={(a, b) => `${a}${b}`} showRefresh="b" on:refresh={refreshEnding}/>

<Equation title="Text Subtraction" summary="Subtract part of the name." bind:a={name} b={name[name.length-1]} op="-" formula={replaceEndings}/>

<Equation title="Adjectives" summary="Add an adjective prefix to the name." bind:a={adjective} bind:b={name} formula={(a, b) => `${a} ${b}`} showRefresh="b" on:refresh={refreshAdjective}/>

<Equation title="Joining" summary="Join multiple words together and remove some parts." bind:a={first} bind:b={second} formula={(a, b) => value}>
  <div class="checklist">
    {#each checked as char, index}
      <label on:click={() => toggleChecked(index)} class:checked={char.checked}>
        {char.letter}
      </label>
    {/each}
  </div>
</Equation>
<Equation title="Removing vowels" summary="Remove one or more vowels from the name" bind:a={name} b={vowels} op="-" formula={(a, b) => a.replace(new RegExp(`${vowels.split('').join('|')}`, 'g'),"")}/>

<footer>
  By <a href="https://twitter.com/joshnuss">@joshnuss</a>
</footer>

<style>
  a, h1 {
    color: aqua;
    text-decoration: underline;
  }
  .checklist label {
    font-size: 2rem;
    display: inline-block;
    padding: 1px;
    cursor: pointer;
    background: red;
    color: white;
  }
  label.checked {
    background: transparent;
    color: black;
  }
  label.checked:hover {
    text-decoration: underline;
  }
</style>
