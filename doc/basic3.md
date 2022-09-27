## Svelte

### basic

#### Dom Events

mousemove
```svelte
<script>
	let m = { x: 0, y: 0 };

	function handleMousemove(event) {
		m.x = event.clientX;
		m.y = event.clientY;
	}
</script>


<div on:mousemove={handleMousemove}>
	The mouse position is {m.x} x {m.y}
</div>

<style>
	div { width: 100%; height: 100%; }
</style>

<script>
	let m = { x: 0, y: 0 };
</script>
// inline 선언
// 일부 프레임워크에서는 특히 루프 내부와 같은 성능상의 이유로 인라인 
// 이벤트 핸들러를 피하라는 권장 사항을 볼 수 있습니다. 
// 그 조언은 Svelte에는 적용되지 않습니다
<div on:mousemove="{e => m = { x: e.clientX, y: e.clientY }}">
	The mouse position is {m.x} x {m.y}
</div>

<style>
	div { width: 100%; height: 100%; }
</style>
```

Dom event handler는 동작을 변경하는 수정자를 가질 수 있음
- once는 한번만 수정가능 (once— 핸들러가 처음 실행된 후 제거)
- preventDefaultevent.preventDefault()— 핸들러를 실행하기 전에 호출
- stopPropagationevent.stopPropagation()— 다음 요소에 도달하는 이벤트를 방지하는 호출
- passive— 터치/휠 이벤트에 대한 스크롤 성능 향상(Svelte는 안전한 위치에 자동으로 추가)
- capture— 버블링 단계 대신 캡처 단계 에서 핸들러를 실행
- self— event.target이 요소 자체인 경우에만 트리거 핸들러
- trusted— 인 경우에만 트리거 핸들러 event.isTrusted 입니다
(true. 즉, 이벤트가 사용자 작업에 의해 트리거된 경우)
수정자를 함께 연결할 수 있습니다(예: on:click|once|capture={...}.


```svelte
<script>
	function handleClick() {
		alert('no more alerts')
	}
</script>

<button on:click|once={handleClick}>
	Click me
</button>
```

dispatch

부모의 컴포넌트를 자식이 사용하기 위해서 사용

```svelte
<script>
	import Inner from './Inner.svelte';

	function handleMessage(event) {
		alert(event.detail.text);
	}
</script>

<Inner on:message={handleMessage}/>

<script>
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();

	function sayHello() {
		dispatch('message', {
			text: 'Hello!'
		});
	}
</script>

<button on:click={sayHello}>
	Click to say hello
</button>
```

하위에 하위의 dispatch

```
<script>
	import Outer from './Outer.svelte';

	function handleMessage(event) {
		alert(event.detail.text);
	}
</script>

<Outer on:message={handleMessage}/>

<script>
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();

	function sayHello() {
		dispatch('message', {
			text: 'Hello!'
		});
	}
</script>

<button on:click={sayHello}>
	Click to say hello
</button>

<script>
	import Inner from './Inner.svelte';
</script>

<Inner on:message/>
```


```svelte
<script>
	import CustomButton from './CustomButton.svelte';

	function handleClick() {
		alert('Button Clicked');
	}
</script>

<CustomButton on:click={handleClick}/>
```

