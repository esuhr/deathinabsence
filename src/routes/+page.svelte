<script>
	import Cover from './Cover.svelte';
	import { stories } from './Stories.svelte'
	import { blur } from 'svelte/transition';
	import { linear } from 'svelte/easing';

	let pageContainer;
	let innerHeight, innerWidth;
	let index = 0;
	let options = { duration: 1800, easing: linear};
	let typed;
	let vis = "visible";
	let story = false;

	const randomize = object => {
		const keysArray = Object.keys(object);
		const length = keysArray.length;
		const result = [];
		const numbersArray = Array.from({length}, (_, i) => i + 1);
		
		for(let i = length - 1; i > 0; i--) {
			const j = Math.floor(Math.random() * (i + 1));
			[numbersArray[i], numbersArray[j]] = [numbersArray[j], numbersArray[i]];
		}
		for(let i = 0; i < length; i++) {
			result.push(numbersArray[i])
		}
		return result;
	}

	const randomStoryIndex = randomize(stories)

	const togglePost = (param) => {
		index = param;
		typed = '';
		let string = stories[param].text;
		let typing;
		let i = 0;

		typing = setInterval(() => {
			if(i < string.length) {
				typed += string[i];
				i++;
			} else {
				clearInterval(typing);
			}
		}, 10);
	}

	
	const toggleVis = () => {
		vis = "hidden";
		story = true;
	}

	$: index;
	$: typed;
</script>

<svelte:window bind:innerHeight={innerHeight} bind:innerWidth={innerWidth}/>

{#if !story}
	<a href="/"on:click|preventDefault={() => toggleVis()}>
		<Cover />
	</a>

	{:else}
		<div class="pageContainer" bind:this={pageContainer}>
			<div class="cutoutContainer" style="height:{innerHeight}; width:{innerWidth};" transition:blur={options} >
				{#each randomStoryIndex as story}
					<div class="cutout" >
						<a class="cutoutLink" href="/"on:click|preventDefault={() => togglePost(story)} >
							<img src="/cutouts/{story}c.webp" alt="">
						</a>
					</div>
				{/each}
				{#if index}
					{#key index}
						<img class="cut" src="/cutouts/{index}.webp" alt="" transition:blur={options}>
					{/key}
					<div class="postContainer" >
						<div class="post">
							<p>{typed}</p>
						</div>
					</div>
				{/if}
			</div>
		</div>
{/if}



<style>

	@import url('https://fonts.googleapis.com/css2?family=Spectral:wght@200&display=swap');
	
	:global(body) {
		margin: 0;
		padding: 0;
		background-color: #e4e0d9;
	}
	.pageContainer {
		display: flex;
		flex-direction: column;
	}
	.cutoutContainer {
		display: flex;
		flex-direction: row;
		width: 100%;
		padding-top: 5vh;
	}
	.cutout img {
		height: 30vh;
	}
	.cutoutLink {
		opacity: 0.5;
	}
	.cutoutLink:hover {
		opacity: 1;
		transition-duration: 1s;
	}
	.postContainer {
		pointer-events: none;
		position: fixed;
		left: 50%;
		top: 35vh;
		font-family: 'Spectral', sans-serif;
		color: #272727;
		transform: translate(-50%,0);
		display: flex;
		flex-direction: column;
		align-items: center;
		height: 80vh;
		width: 100vw;
		z-index: 1;
		border: 1px red solid;
	}
	.cut {
		position: fixed;
		min-height: 30vmax;
		max-height: 50vh;
		min-width: 5vw;
		z-index: 10;
		top: 40vh;
		left: 50%;
		transform: translate(-50%, 0);
		
	}
	.post {
		font-size: 4vmin;
		width: 100vw;
		line-height: 4vh;
		overflow: hidden;
		

	}
	.post p {
		top: 0;
		padding: 5vmax;
	}
</style>