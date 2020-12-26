<script>

	//imports
	import { tweened } from 'svelte/motion';
	import { shuffleArray } from "./functions";
	import { cubicOut, circOut } from 'svelte/easing';

	//components
	import Wave from './components/Wave.svelte'
	import TheBook from './components/TheBook.svelte'
	
	
	//vars
	let books = localStorage.getStuff('unreadBooks')
	let animationEnded = false
	let bookHasBeenRead = false
	let audion = new Audio('./assets/dsshotgn.wav')
	let waving = false

	// countdown
	const progress = tweened(0, {
		duration: 2500,
		easing: circOut
	})

	//$progress.

	// functions
	const chooseBook = () => {
		books =  localStorage.getStuff('unreadBooks')
		shuffleArray(books)

		console.log(books)

		bookHasBeenRead = false
		animationEnded = false

		let index = $progress === 0 ? books.length - 1 : 0

		progress.set(index).then(() => {
			animationEnded = true
			audion.play()
		})
		
	}
	const readBook = () => {
		let readBooks = localStorage.getStuff('readBooks')
		let unreadBooks = localStorage.getStuff('unreadBooks')

		bookHasBeenRead = true

		readBooks.push(chosenBook)
		unreadBooks = unreadBooks.filter(item => item.id !== chosenBook.id)

		localStorage.setStuff('readBooks', readBooks)
		localStorage.setStuff('unreadBooks', unreadBooks)
	}

	$: chosenBook = books[Math.floor($progress)] || books[0]
</script>

<style>
	label {
		position: absolute;
		top: 1em;
		left: 1em;
		font-size: 0.7em;
	}

</style>

<header>
	<h1>HEMINGVAYOVÁTORO-ROTÁTOR</h1> 


	<label for="waving"><input type="checkbox" id="waving" bind:checked={waving}>
		waving
	</label>
</header>

<main>
	
	<TheBook {chosenBook} {animationEnded} {bookHasBeenRead} />

	
	<section class='controls'>
		<img on:click={chooseBook} src="./assets/gun.png" alt="gun">
		<img on:click={readBook} src="./assets/anchor.png" alt="anchor">
	</section>
</main>

<footer class='water'>
	<Wave where='back' animation='big' {waving}/>
	<Wave where='middle' animation='tiny' {waving}/>
	<img class="hem" src="./assets/hemingway.png" alt="ernie"/>
	<Wave where='front' animation='small' {waving}/>
	
</footer>
