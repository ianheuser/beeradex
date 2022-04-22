
<script>
    /// Import global css styles, the contentful library to bring our data in, and the onMount function from svelte
    ///import {gsap} from "gsap"
    import '../styles/global_2.css'; 
    import * as contentful from "contentful"
    import { onMount } from 'svelte';
    /// import Slider from '$lib/Slider.svelte';

    /// create variables to store brands, products of the current brand, and the state of the menu (open or closed)
    let allBrands = [];
    let products = [];

    // onMount svelte function runs once the component or page in question has mounted (similar to onLoad or document.ready() )
    onMount(async() => {
            
        document.querySelector('.top').classList.add('open');
        document.querySelector('.bottom').classList.add('open');

        // select and store the "switch brands" button and the brands menu, to create the brand menu functionality 
        // personal choice, I like to use dollar signs as 1st character of any variable name that stores a dom element
        
        let $brandMenu =  document.querySelector('.brands');

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

    });

    
    let cur = 0;
        
        function changeSlide(slide) {
            cur = slide;
        }
        
        const clamp = (number, min, max) => Math.min(Math.max(number, min), max);

        function prev(e) {
            cur = clamp( --cur, 0, products.length-1 )
        }
        
        //
        function next(e) {
            cur = clamp( ++cur, 0, products.length-1 )
        }
        
        const ARROW_LEFT = 37;
        const ARROW_RIGHT = 39;

        function handleShortcut(e) {
            if (e.keyCode === ARROW_LEFT ) {
                prev();
            }
            if (e.keyCode === ARROW_RIGHT ) {
                next();
            }
        }

</script>

<section id="main">
    <div class="ribbon top"></div>
    <div class="belly">
        <aside class="left">
            <div class="logoHolder">
                <img src="./img/logos/our-beers/Budweiser.png" alt="Budweiser Logo" class="logo"/>
            </div>
            <div class="productLinks">
                <button class="productLink current" href="www.budweiser.com">Budweiser</button>
                <button class="productLink" href="www.budweiser.com">Select</button>
                <button class="productLink" href="www.budweiser.com">Select 55</button>
                <button class="productLink" href="www.budweiser.com">Zero</button>
                <button class="productLink" href="www.budweiser.com">Chelada</button>
                <button class="productLink" href="www.budweiser.com">Supreme</button>
            </div>
        </aside>
        <div class="right">
            <div class="brandsMask">
                <div class="brands">
                    {#each allBrands as oneBrand }
                        <div class="brand">
                        {#if oneBrand.fields.logo.fields.file.url }
                            <img class="brandLogo" src={oneBrand.fields.logo.fields.file.url} alt={ "The logo for " + oneBrand.fields.name } />
                        {/if}
                        </div>
                    {/each}
                </div>
            </div>
            <div class="page">

           
                <!--
                <div class="information slide">
                    <h1 class="title">Budweiser</h1>
                    <p class="blurb">
                        Budweiser is a medium-bodied, flavorful, crisp American-style lager. Brewed with the best barley malt and a blend of premium hop varieties.
                    </p>
                    <div class="nutrition">
                        <div class="metrics">
                            <h5>ALC%</h5>
                            <h2>5</h2>
                            <h5>BY VOL</h5>
                        </div>
                        <div class="metrics">
                            <h5>CAL</h5>
                            <h2>145</h2>
                            <h5>PER 12 OZ</h5>
                        </div>
                        <div class="metrics">
                            <h5>FAT</h5>
                            <h2>0</h2>
                            <h5>PER 12 OZ</h5>
                        </div>
                        <div class="metrics">
                            <h5>CARBS</h5>
                            <h2>10.6</h2>
                            <h5>PER 12 OZ</h5>
                        </div>
                    </div>
                </div-->

   
			    {#each products as product, id}
                    {#if id === cur}
                        <div class="slide">
                            <div class="text-content">
                                <h1 class="title">{product.fields.name}</h1>
                                <p class="text">{product.fields.extendedBlurb}</p>
                                <div class="spec-content">
                                    <div class="box">
                                        <h3 class="box-spec">ALC%</h3>
                                        <h2>{product.fields.alcoholPercent}</h2>
                                        <h3 class="box-text">BY VOL</h3>
                                    </div>
                                    <div class="box">
                                        <h3 class="box-spec">CAL</h3>
                                        <h2>{product.fields.caloriesPer12Oz}</h2>
                                        <h3 class="box-text">PER 12 OZ</h3>
                                    </div>
                                    <div class="box">
                                        <h3 class="box-spec">FAT</h3>
                                        <h2>{product.fields.fat}</h2>
                                        <h3 class="box-text">PER 12 OZ</h3>
                                    </div>

                                    <div class="box">
                                        <h3 class="box-spec">CARBS</h3>
                                        <h2>{product.fields.carbs}</h2>
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

            </div>
        </div>
    </div>
    <div class="ribbon bottom"></div>
    
</section>
