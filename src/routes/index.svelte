
<script>
    /// Import global css styles, the contentful library to bring our data in, and the onMount function from svelte
    ///import {gsap} from "gsap"
    import '../styles/global.css'; 
    import * as contentful from "contentful"
    import { onMount } from 'svelte';
    /// import Slider from '$lib/Slider.svelte';

    /// create variables to store brands, products of the current brand, and the state of the menu (open or closed)
    let allBrands = [];
    let menuState = 'closed';
    let products = [];

    // onMount svelte function runs once the component or page in question has mounted (similar to onLoad or document.ready() )
    onMount(async() => {
            
        document.querySelector('.top').classList.add('open');
        document.querySelector('.bottom').classList.add('open');

        // select and store the "switch brands" button and the brands menu, to create the brand menu functionality 
        // personal choice, I like to use dollar signs as 1st character of any variable name that stores a dom element
        let $switchBrand =  document.querySelector('.switchBrands');
        let $closeBrands =  document.querySelector('.closeButton');
        let $brandMenu =  document.querySelector('.brands');

        //add click event listener to the "switch brands" button
        $switchBrand.addEventListener('click', function(){
                $brandMenu.classList.add('openMenu');
        });
        $closeBrands.addEventListener('click', function(){
                $brandMenu.classList.remove('openMenu');
        });

        //Connect to our contentful account data, store the connection in the variable "client"
        const client = contentful.createClient({
            space: "3q892y4ckspg",
            accessToken: "w_ghIFLSyjNtCW4BdthHMn8WH21jXSC54suwYdXvxxQ",
            environment: "master"
        });

        //use contentful client and getEntries method to bring back all the brands
        client.getEntries({
            content_type: "brand"
        }).then(response => {
            allBrands = response.items;
        }).catch(error => {
            console.log("Error in Brand Pull back");
            console.dir(error);
        });

        //use contentful client and getEntries method to bring back all the products
        client.getEntries({
            content_type: "product",
            "fields.brand.sys.id": "2CVZpj6Pglkkylk8xOjBhw",
            "fields.type" : "Beer"
        }).then(response => {
            products = response.items;
            console.log('THE PRODUCTS:')
            console.dir(products)
        }).catch(error => {
            console.log("Error in Product pull");
            console.dir(error);
        });
/*
        const tl = gsap.timeline();
        const duration = 1;

        tl.from('.bottom', {
            duration,
            y: +500,
            ease: 'out',
        })
        .from('.top', {
            duration,
            y: -500,
            ease: 'out',
        }, `-=${duration * 0.75}`)
*/
    });

	/*
        let slides = [
		{ title: 'BUDWEISER', text: 'Budweiser is a medium-bodied, flavorful, crisp American-style lager. Brewed with the best barley malt and a blend of premium hop varieties.', t1a:'ALC %', t1b:'CAL', t1c:'FAT', t1d:'CARBS',t2a:'5', t2b:'145', t2c:"0",t2d:'10.6', t3a:'BY VOL', t3b:'PER 12 OZ', t3c:'PER 12 OZ',t3d:'PER 8 OZ', image:'https://us.budweiser.com/img/home/desktop/02_Iconic_12ozBottle_T_529x654.png' },
		{ title: 'BUDWEISER SUPREME', text: 'Budweiser is a medium-bodied, flavorful, crisp American-style lager. Brewed with the best barley malt and a blend of premium hop varieties.', t1a:'ALC %', t1b:'CAL', t1c:'FAT', t1d:'CARBS',t2a:'15', t2b:'155', t2c:"0",t2d:'10.6', t3a:'BY VOL', t3b:'PER 12 OZ', t3c:'PER 12 OZ',t3d:'PER 8 OZ', image:'https://us.budweiser.com/img/home/desktop/15_Supreme_12ozBottle_T_529x654.png' },
		{ title: 'BUDWEISER ZERO', text: 'Budweiser is a medium-bodied, flavorful, crisp American-style lager. Brewed with the best barley malt and a blend of premium hop varieties.', t1a:'ALC %', t1b:'CAL', t1c:'FAT', t1d:'CARBS',t2a:'25', t2b:'45', t2c:"0",t2d:'10.6', t3a:'BY VOL', t3b:'PER 12 OZ', t3c:'PER 12 OZ',t3d:'PER 8 OZ', image: 'https://us.budweiser.com/img/home/desktop/Zero-12ozCan.png'},
            
        ]
        */
        //current slide variable
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
            cur = clamp( --cur, 0, products.length-1 )
        }
        
        //
        function next(e) {
            cur = clamp( ++cur, 0, products.length-1 )
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
<section id="main">
    <div class="brands">
        {#each allBrands as oneBrand }
            <div class="brand">
               {#if oneBrand.fields.logo.fields.file.url }
                    <img class="brandLogo" src={oneBrand.fields.logo.fields.file.url} alt={ "The logo for " + oneBrand.fields.name } />
               {/if}
            </div>
        {/each}
        <img src="./img/closeButton.png" width="30" height="30" alt="Close menu icon" class="closeButton" />
    </div>
    <div class="top">
        <div class="primary"></div>
        <div class="header secondary">
            <img class="logo" src="./img/logos/our-beers/Budweiser.png" alt="The logo for Budweiser" />
            <div class="switchBrands">
                <span>Switch Brands</span>
                <img class="brandIcon" src="./img/menu.png" alt="Menu icon" />
            </div>
        </div>
    </div>
   
<div class="extra-outer-wrapper">
	<div class="outer-wrapper">
		<div class="inner-wrapper">
            
            <button class="nextAndBack back" on:click="{()=>prev()}">
                &lt;
            </button>
                   
			{#each products as product, id}
				{#if id === cur}
                    
			<div class="slide">
					<div class="text-content">
						<h1 class="title">{product.fields.name}</h1>
						<p class="text">{product.fields.extendedBlurb}</p>
						<div class="spec-content">
							<div class="box">
								<h3 class="box-spec">ALC%</h3>
								<h1>{product.fields.alcoholPercent}</h1>
								<h3 class="box-text">BY VOL</h3>
							</div>
							<div class="box">
								<h3 class="box-spec">CAL</h3>
								<h1>{product.fields.caloriesPer12Oz}</h1>
								<h3 class="box-text">PER 12 OZ</h3>
							</div>
							<div class="box">
								<h3 class="box-spec">FAT</h3>
								<h1>{product.fields.fat}</h1>
								<h3 class="box-text">PER 12 OZ</h3>
							</div>

							<div class="box">
								<h3 class="box-spec">CARBS</h3>
								<h1>{product.fields.carbs}</h1>
								<h3 class="box-text">PER 8 OZ</h3>
							</div>
						</div>
					</div>
					<div class="productImage">
						<img src="{product.fields.profileImage.fields.file.url}" alt="">
					</div>
                    
                    
				</div>
				{/if}
			{/each}
            <button class="nextAndBack next" on:click="{()=>next()}">
                &gt;
            </button>
			
	
		</div>
	</div>
</div>

<div class="bottom">
	<div class="products secondary">
		<div class="productLinks">
			<div class="productLinks">
                <!--
                <button class="productLink current" href="www.budweiser.com">Budweiser</button>
                <button class="productLink" href="www.budweiser.com">Select</button>
                <button class="productLink" href="www.budweiser.com">Select 55</button>
                <button class="productLink" href="www.budweiser.com">Zero</button>
                <button class="productLink" href="www.budweiser.com">Chelada</button>
                <button class="productLink" href="www.budweiser.com">Supreme</button>
                    -->
                {#each products as product, i}
                    <button on:click={()=>changeSlide(i)} class="productLink" class:current={cur == i}>{product.fields.name}</button>
                {/each}
            </div>
		</div>
	</div>
	<div class="veryBottom primary">
		
	</div>
</div>
    
    
    
</section>
