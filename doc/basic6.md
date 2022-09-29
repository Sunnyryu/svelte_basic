## Svelte

### basic

#### LifeCycle

onMount

```svelte
<script>
	import { onMount } from 'svelte';

	let photos = [];

	onMount(async () => {
		const res = await fetch(`/tutorial/api/album`);
		photos = await res.json();
	});
</script>

<h1>Photo album</h1>

<div class="photos">
	{#each photos as photo}
		<figure>
			<img src={photo.thumbnailUrl} alt={photo.title}>
			<figcaption>{photo.title}</figcaption>
		</figure>
	{:else}
		<!-- this block renders when photos.length === 0 -->
		<p>loading...</p>
	{/each}
</div>

<style>
	.photos {
		width: 100%;
		display: grid;
		grid-template-columns: repeat(5, 1fr);
		grid-gap: 8px;
	}

	figure, img {
		width: 100%;
		margin: 0;
	}
</style>

//onMount 컴포넌트가 처음 DOM에 랜더링 된 후에 실행 되옵니다. / 
//라이프 사이클 함수는 컴포넌트 초기화 중에 호출하여 콜백이 컴포넌트 인스턴스에 바인드되도록 해야함 (EX: setTimeout)
```

onDestroy

```svelte
<script>
	import Timer from './Timer.svelte';

	let open = true;
	let seconds = 0;

	const toggle = () => (open = !open);
	const handleTick = () => (seconds += 1);
</script>

<div>
	<button on:click={toggle}>{open ? 'Close' : 'Open'} Timer</button>
	<p>
		The Timer component has been open for
		{seconds} {seconds === 1 ? 'second' : 'seconds'}
	</p>
	{#if open}
	<Timer callback={handleTick} />
	{/if}
</div>

// 코드가 실행된 뒤에 컴포넌트를 파괴ㅣ킴
```

```svelte

<script>
	import Eliza from 'elizabot';
	import { beforeUpdate, afterUpdate } from 'svelte';

	let div;
	let autoscroll;

	beforeUpdate(() => {
		autoscroll = div && (div.offsetHeight + div.scrollTop) > (div.scrollHeight - 20);
	});

	afterUpdate(() => {
		if (autoscroll) div.scrollTo(0, div.scrollHeight);
	});

	const eliza = new Eliza();

	let comments = [
		{ author: 'eliza', text: eliza.getInitial() }
	];

	function handleKeydown(event) {
		if (event.key === 'Enter') {
			const text = event.target.value;
			if (!text) return;

			comments = comments.concat({
				author: 'user',
				text
			});

			event.target.value = '';

			const reply = eliza.transform(text);

			setTimeout(() => {
				comments = comments.concat({
					author: 'eliza',
					text: '...',
					placeholder: true
				});

				setTimeout(() => {
					comments = comments.filter(comment => !comment.placeholder).concat({
						author: 'eliza',
						text: reply
					});
				}, 500 + Math.random() * 500);
			}, 200 + Math.random() * 200);
		}
	}
</script>

<style>
	.chat {
		display: flex;
		flex-direction: column;
		height: 100%;
		max-width: 320px;
	}

	.scrollable {
		flex: 1 1 auto;
		border-top: 1px solid #eee;
		margin: 0 0 0.5em 0;
		overflow-y: auto;
	}

	article {
		margin: 0.5em 0;
	}

	.user {
		text-align: right;
	}

	span {
		padding: 0.5em 1em;
		display: inline-block;
	}

	.eliza span {
		background-color: #eee;
		border-radius: 1em 1em 1em 0;
	}

	.user span {
		background-color: #0074D9;
		color: white;
		border-radius: 1em 1em 0 1em;
		word-break: break-all;
	}
</style>

<div class="chat">
	<h1>Eliza</h1>

	<div class="scrollable" bind:this={div}>
		{#each comments as comment}
			<article class={comment.author}>
				<span>{comment.text}</span>
			</article>
		{/each}
	</div>

	<input on:keydown={handleKeydown}>
</div>
///beforeupdate 함수는 DOM이 업데이트되기 바로 전에 실행되도록 작업을 예약합니다. 
///afterupdate는 DOM이 데이터와 동기화되면 코드를 실행하는 데 사용

```

tick
```svelte

<script>
	import { tick } from 'svelte';

	let text = `Select some text and hit the tab key to toggle uppercase`;

	async function handleKeydown(event) {
		if (event.key !== 'Tab') return;

		event.preventDefault();

		const { selectionStart, selectionEnd, value } = this;
		const selection = value.slice(selectionStart, selectionEnd);

		const replacement = /[a-z]/.test(selection)
			? selection.toUpperCase()
			: selection.toLowerCase();

		text = (
			value.slice(0, selectionStart) +
			replacement +
			value.slice(selectionEnd)
		);

		await tick();
		this.selectionStart = selectionStart;
		this.selectionEnd = selectionEnd;
	}
</script>

<style>
	textarea {
		width: 100%;
		height: 200px;
	}
</style>

<textarea value={text} on:keydown={handleKeydown}></textarea>

```