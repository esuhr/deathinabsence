# death in absence

This project uses a lottie animation for the title animation and svelte transitions for most of the other animations (blur, fade).

The other effects--randomizing story order and the typewriter effect for text--are accomplished through the randomize function and the togglePost function.

The randomize function takes an object (e.g. stories = { 1: {text: "i met ...}}) and returns a randomly ordered array of objects.
The togglePost function takes a parameter (the index of the photo clicked by the user) and displays the linked text and image. Currently, the image names are used as an index; a more complete solution would be to place paths to the images in the story object. This approach could be adapted to work with other blogging platforms with an API (e.g. Wordpress and GraphQL).

```
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
```
