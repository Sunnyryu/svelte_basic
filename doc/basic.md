## Svelte

### basic

#### tutorial
```svelte
<h1>What now?</h1>

<script>
	import Nested from './Nested.svelte';
	let string = `he is  <strong>dancer</strong>`;
		let src = '/tutorial/image.gif';
	let name = 'Rick Astley';
</script>
<Nested/>
<p class="abc">{@html string}  and he name is <strong class="bbq">Ricky Martin</strong></p>
<img {src} alt="{name} dances.">



<style>
	.abc > .bbq {
		color: blue;
		font-family: 'Comic Sans MS', cursive;
		font-size: 2em;
	}
	
</style>

```

#### reactive
count button

```svelte
<script>
	let count = 0;

	function incrementCount() {
		count += 1;
		if (count > 20){
			count = 0;
		}
	}
</script>

<button on:click={incrementCount}>
	Clicked {count} {count === 1 ? 'time' : 'times'}
</button>

```

```svelte
<script>
	let count = 0;
	$: doubled = count * 2;

	function handleClick() {
		count += 1;
		if (count > 20){
			count = 0;
		}
	}
</script>

<button on:click={handleClick}>
	Clicked {count} {count === 1 ? 'time' : 'times'}
</button>

<p>{count} doubled is {doubled}</p>
```

```svelte
<script>
	let count = 0;

	$: if (count >= 100) {
		alert('count is dangerously high!');
		count = 9;
	}

	function handleClick() {
		count += 1;
	}
</script>

<button on:click={handleClick}>
	Clicked {count} {count === 1 ? 'time' : 'times'}
</button>
```

```svelte
<script>
	let numbers = [1];

	function addNumber() {
		numbers = [...numbers, numbers.length + 1];
	}

	$: sum = numbers.reduce((t, n) => t + n, 0);
	// t = previous number
	// n = current number
	// 0 = initial number
</script>

<p>{numbers.join(' + ')} = {sum}</p>

<button on:click={addNumber}>
	Add a number
</button>
```

#### props

```svelte
//App.svelte
<script>
	import Nested from './Nested.svelte';
</script>

<Nested answer={42}/>
//Nested.svelte
<script>
	export let answer;
</script>

<p>The answer is {answer}</p>
```

```
<script>
	import Nested from './Nested.svelte';
</script>

<Nested answer={42}/>
<Nested/>

<script>
	export let answer = 'a mystery';
</script>

<p>The answer is {answer}</p>
```

```
<script>
	import Info from './Info.svelte';

	const pkg = {
		name: 'svelte',
		version: 3,
		speed: 'blazing',
		website: 'https://svelte.dev'
	};
</script>

<Info {...pkg}/>
//<Info name={pkg.name} version={pkg.version} speed={pkg.speed} website={pkg.website}/>

<script>
	export let name;
	export let version;
	export let speed;
	export let website;
</script>

<p>
	The <code>{name}</code> package is {speed} fast.
	Download version {version} from <a href="https://www.npmjs.com/package/{name}">npm</a>
	and <a href={website}>learn more here</a>
</p>
```