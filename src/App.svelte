<script>
  import Equation from './Equation.svelte'
  import animals from './animals.js'
  import adjectives from './adjectives.js'
  import colors from './colors.js'
  import { onMount } from 'svelte'

  const names = [
    'cart',
    'payment',
    'order',
    'mail',
    'ship',
    'box',
  ]
  const endings = [
    'ify',
    'ly',
    'io',
    'ium',
    'r',
    'app',
    'club',
    'hq',
    'please',
  ]
  const prefixes = [
    'check',
    'club',
    'do',
    'get',
    'give',
    'go',
    'grab',
    'hello',
    'hey',
    'join',
    'lets',
    'the',
    'try',
    'use',
    'wear',
    'why',
  ]
  const vowels = 'aeiou'

  let name = getRandomItem(names)
  let animal = 'panda'
  let ending = endings[getRandomInt(endings.length)]
  let prefix = prefixes[getRandomInt(prefixes.length)]
  let color = colors[getRandomInt(colors.length)]
  let adjective = 'smart'
  let first = 'accent', second = 'future'
  let checked = []
  let subtraction = name[name.length-1]

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
    animal = getRandomItem(animals, animal)
  }

  function refreshEnding() {
    ending = getRandomItem(endings, ending)
  }

  function refreshPrefix() {
    prefix = getRandomItem(prefixes, prefix)
  }

  function refreshAdjective() {
    adjective = getRandomItem(adjectives, adjective)
  }

  function refreshColor() {
    color = getRandomItem(colors, color)
  }

  function getRandomItem(list, current) {
    const index = getRandomInt(list.length)
    const item = list[index]

    if (item == current)
      return getRandomItem(list, current)

    return item
  }

  function getRandomInt(max) {
    return Math.floor(Math.random() * Math.floor(max))
  }

  function replaceEndings(a, b) {
    let replaced = a.replace(new RegExp(`${b}$`), '')
    if (replaced != a) return replaced

    replaced = a.replace(new RegExp(`^${b}`), '')
    if (replaced != a) return replaced

    return a.replace(new RegExp(b, 'g'), '')
  }

  function toggleChecked(index) {
    checked[index].checked = !checked[index].checked
    checked = checked
  }
</script>

<!-- TODO add local storage store -->

<div class="container">

  <header>
    <h1>🐼 Name Panda</h1>
    <p>
    Generate a name for your SaaS product. Inspired by <a href="https://twitter.com/Baremetrics/status/1276543395008307201">@baremetric's tweet</a>
    </p>
  </header>


<Equation title="Animals" summary="Add an animal to the end of a name." bind:a={name} bind:b={animal} labelA="Name" labelB="Animal" formula={(a, b) => `${a} ${b}`} showRefresh="b" on:refresh={refreshAnimal}/>

<Equation title="Appending" summary="Append a short suffic to your name." bind:a={name} bind:b={ending} labelA="Name" labelB="Ending" formula={(a, b) => `${a}${b}`} showRefresh="b" on:refresh={refreshEnding}/>

<Equation title="Prepending" summary="Prepend a short prefix to your name." bind:a={prefix} bind:b={name} labelA="Prefix" labelB="Name" formula={(a, b) => `${a}${b}`} showRefresh="a" on:refresh={refreshPrefix}/>

<Equation title="Text Subtraction" summary="Subtract part of the name." bind:a={name} b={subtraction} labelA="Name" labelB="Pattern" op="-" formula={replaceEndings}/>

<Equation title="Adjectives" summary="Add an adjective prefix to the name." bind:a={adjective} bind:b={name} labelA="Adjective" labelB="Name" formula={(a, b) => `${a} ${b}`} showRefresh="a" on:refresh={refreshAdjective}/>

<Equation title="Colors" summary="Add a color prefix to the name." bind:a={color} bind:b={name} labelA="Color" labelB="Name" formula={(a, b) => `${a} ${b}`} showRefresh="a" on:refresh={refreshColor}/>

<Equation title="Joining" summary="Join multiple words together and remove some parts." bind:a={first} bind:b={second} labelA="First word" labelB="Second word" formula={() => value}>
  <div class="checklist">
    {#each checked as char, index}
      <label on:click={() => toggleChecked(index)} class:checked={char.checked}>
        {char.letter}
      </label>
    {/each}
  </div>
</Equation>
<Equation title="Removing vowels" summary="Remove one or more vowels from the name" bind:a={name} b={vowels} op="-" labelA="Name" labelB="Vowels" formula={(a, b) => a.replace(new RegExp(`${b.split('').map(x => '(' + x + ')').join('|')}`, 'g'),'')}/>

<footer>
  Site by <a href="https://twitter.com/joshnuss">@joshnuss</a>. Inspired by <a href="https://twitter.com/Baremetrics/status/1276543395008307201">@baremetric's tweet</a>.
</footer>
</div>

<style>
  :global(body) {
    background: cornflowerblue;
  }
  h1 {
    margin: 2em 0 0.5em;
  }
  p {
    color: white;
  }
  p a {
    font-weight: bold;
  }

  header {
    margin-bottom: 3rem;
  }

  a, h1 {
    color: white;
  }

  a {
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
    background: pink;
  }

  footer {
    color: #eee;
    font-size: 0.9em;
    margin-bottom: 2rem;
  }

  @media only screen and (min-width: 768px) {
    .container {
      min-width: 900px;
      max-width: 60vw;
      margin: auto;
    }
  }
</style>
