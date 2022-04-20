
<script>
	/*
import { slide } from "svelte/types/runtime/transition";
*/

	export const products = [];

	let slides = [
		{ title: 'BUDWEISER', text: 'Budweiser is a medium-bodied, flavorful, crisp American-style lager. Brewed with the best barley malt and a blend of premium hop varieties.', t1a:'ALC %', t1b:'CAL', t1c:'FAT', t1d:'CARBS',t2a:'5', t2b:'145', t2c:"0",t2d:'10.6', t3a:'BY VOL', t3b:'PER 12 OZ', t3c:'PER 12 OZ',t3d:'PER 8 OZ', image:'https://us.budweiser.com/img/home/desktop/02_Iconic_12ozBottle_T_529x654.png' },
		{ title: 'BUDWEISER SUPREME', text: 'Budweiser is a medium-bodied, flavorful, crisp American-style lager. Brewed with the best barley malt and a blend of premium hop varieties.', t1a:'ALC %', t1b:'CAL', t1c:'FAT', t1d:'CARBS',t2a:'15', t2b:'155', t2c:"0",t2d:'10.6', t3a:'BY VOL', t3b:'PER 12 OZ', t3c:'PER 12 OZ',t3d:'PER 8 OZ', image:'https://us.budweiser.com/img/home/desktop/15_Supreme_12ozBottle_T_529x654.png' },
		{ title: 'BUDWEISER ZERO', text: 'Budweiser is a medium-bodied, flavorful, crisp American-style lager. Brewed with the best barley malt and a blend of premium hop varieties.', t1a:'ALC %', t1b:'CAL', t1c:'FAT', t1d:'CARBS',t2a:'25', t2b:'45', t2c:"0",t2d:'10.6', t3a:'BY VOL', t3b:'PER 12 OZ', t3c:'PER 12 OZ',t3d:'PER 8 OZ', image: 'https://us.budweiser.com/img/home/desktop/Zero-12ozCan.png'},
		
	]
	
	let cur = 0;
	
	function changeSlide(slide) {
		cur = slide;
	}
	
	const clamp = (number, min, max) => Math.min(Math.max(number, min), max);
	
	/*
	const transition_args = {
		duration: 200,
	}
	*/
	function prev(e) {
		cur = clamp( --cur, 0, slides.length-1 )
	}
	
	//
	function next(e) {
		cur = clamp( ++cur, 0, slides.length-1 )
	}
	
	// set left arrow and right arrow to numbers that represent the key codes for the left and right buttons
  	const ARROW_LEFT = 37;
	const ARROW_RIGHT = 39;

	// function to allow the left and right keys to control the slider
	function handleShortcut(e) {
        if (e.keyCode === ARROW_LEFT ) {
			prev();
        }
		if (e.keyCode === ARROW_RIGHT ) {
            next();
        }
    }

</script>

<svelte:window on:keyup={handleShortcut} />

<div class="extra-outer-wrapper">
	<div class="outer-wrapper">
		<div class="inner-wrapper">
			{#each slides as slide, id}
				{#if id === cur}
				<div class="slide">
					<div class="text-content">
						<h1 class="title">{slide.title}</h1>
						<p class="text">{slide.text}</p>
						<div class="spec-content">
							<div class="box">
								<h3 class="box-spec">{slide.t1a}</h3>
								<h1>{slide.t2a}</h1>
								<h3 class="box-text">{slide.t3a}</h3>
							</div>
							<div class="box">
								<h3 class="box-spec">{slide.t1b}</h3>
								<h1>{slide.t2b}</h1>
								<h3 class="box-text">{slide.t3b}</h3>
							</div>
							<div class="box">
								<h3 class="box-spec">{slide.t1b}</h3>
								<h1>{slide.t2c}</h1>
								<h3 class="box-text">{slide.t3c}</h3>
							</div>

							<div class="box">
								<h3 class="box-spec">{slide.t1d}</h3>
								<h1>{slide.t2d}</h1>
								<h3 class="box-text">{slide.t3d}</h3>
							</div>
						</div>
					</div>
					<div class="image">
						<img src="{slide.image}" alt="">
					</div>
				</div>
				{/if}
			{/each}
			<div class="controls">
				<button on:click="{()=>prev()}">
					&lt;
				</button>
				<button on:click="{()=>next()}">
					&gt;
				</button>
			</div>
		</div>
	</div>
</div>
<!---
<div class="dots">
	{#each slides as slide, i}
		<button on:click={()=>changeSlide(i)} class="dot" class:selected={cur == i}>{i+1}</button>
	{/each}
</div>
-->
<div class="bottom">
	<div class="products secondary">
		<div class="productLinks">
			<!--
			<button class="productLink current" href="www.budweiser.com">Budweiser</button>
			<button class="productLink" href="www.budweiser.com">Select</button>
			<button class="productLink" href="www.budweiser.com">Select 55</button>
			<button class="productLink" href="www.budweiser.com">Zero</button>
			<button class="productLink" href="www.budweiser.com">Chelada</button>
			<button class="productLink" href="www.budweiser.com">Supreme</button>
				-->
			{#each slides as slide, i}
				<button on:click={()=>changeSlide(i)} class="productLink" class:current={cur == i}>{slide.title}</button>
			{/each}
		</div>
	</div>
	<div class="veryBottom primary">
		
	</div>
</div>
<style>
	/*
	:global(html) {
		font-size: 62.5%;
	}
	
	:global(body) {
		font-size: 1.4rem;
	}

	button {
		background: transparent;
		color: #000000;
		border-color: transparent;
		width: 3.2rem;
		height: 3.2rem;
	}
	
	button:hover,
	button:focus{
		background: rgba(0,0,0,0.5);
	}
	
	.dots {
		display: flex;
		align-items: center;
		justify-content: center;
		margin-top: 8px;
	}
	
	.dot {
		width: 8px;
		height: 8px;
		background: #000;
		border-radius: 100%;
		font-size: 0;
		margin: 0.3rem;
		opacity: 0.3;
	}
	
	.dot.selected {
		opacity: 1;
	}*/

	.extra-outer-wrapper {
		width: 100%;

		
	}

	.outer-wrapper {
		width: 100%;
		padding: 0 0 56.25%;
		position: relative;
        margin: auto;
	}
	
	.inner-wrapper {
		height: 100%;
		width: 100%;
		display: flex;
		position: absolute;
        align-items: center;
        justify-content: center;
     
       
	}

	.controls button:first-child {
		position: absolute;
		left: 0;
		top: calc(50% - 1.2rem);
	}
	
	.controls button:last-child {
		position: absolute;
		right: 0;
		top: calc(50% - 1.2rem);
	}
/*	
	.slide {
		flex: 1 0 auto;
		width: 100%;
		height: 100%;
		background: red;
	  align-items: center;
		justify-content: center;
		display: flex;
		text-align: center;
		font-weight: bold;
		font-size: 2rem;
		color: white;
	}

    */
	
	.controls {
		text-align: center;
		width: 100%;
		display: block;
	}

    /* Slide Content */


    

.slide {
    
    display: flex;
    width: 85%;
    flex: 1 0 auto;
    height: 100%;
    padding-left: 80px;
    
   
}

.title {
    font-family: 'Roboto', sans-serif;
    font-weight: 700;
    padding-top: 160px;
}

.text {
    font-family: 'Roboto', sans-serif;
    color: gray;
    padding-top: 20px;
    line-height: 2.5;
}

.spec-content {
    display: flex;
}
.box {
    margin-right: 50px;
    text-align: center;
}

.box-spec, .box-text {
    color: gray;
}


.productLinks{
    position: relative;
    position: relative;
    display: flex;
    align-items: center;
    justify-content: center;
}

.productLink {
    padding: 5px 20px;
    position: relative;
    height: 40px;
    background: #FFFFFF;
    border: 0.5px solid #9F9F9F;
    box-sizing: border-box;
    text-transform: uppercase;
    font-size: 15px;
    margin: 30px 5px;
}

.productLink:hover {
    opacity: .7;
    cursor: pointer;
}
button {
	cursor: pointer;
}
.productLink.current {
    color:#D02030;
    border-color:#D02030;
}


/* Slide Content End */
</style>