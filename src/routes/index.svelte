
<script>
    import '../styles/global.css'; 
    import * as contentful from "contentful"
    import { onMount } from 'svelte';

    let allBrands = [];
    let menuState = 'closed';

    onMount(async() => {

        let $switchBrand =  document.querySelector('.switchBrands');
        let $brandMenu =  document.querySelector('.brands');

        $switchBrand.addEventListener('click', function(){
            if(menuState=='closed'){
                $brandMenu.classList.add('openMenu');
                console.log('CLOSED');
                menuState = 'open';
            } else {
                $brandMenu.classList.remove('openMenu');
                console.log('OPEN');
                menuState = 'closed';
            }
        });

        const client = contentful.createClient({
            space: "3q892y4ckspg",
            accessToken: "w_ghIFLSyjNtCW4BdthHMn8WH21jXSC54suwYdXvxxQ",
            environment: "master"
        });

        client.getEntries({
            content_type: "brand"
        }).then(response => {
            allBrands = response.items;
        }).catch(error => {
            console.log("Error in Brand Pull back");
            console.dir(error);
        });

    });



</script>

<section id="main">
    <div class="brands">
        {#each allBrands as oneBrand }
            <div class="brand">
               {#if oneBrand.fields.logo.fields.file.url }
                    <img class="brandLogo" src={oneBrand.fields.logo.fields.file.url} alt={ "The logo for " + oneBrand.fields.name } />
               {/if}
            </div>
        {/each}
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
    <div class="middle">  

 
    </div>
    <div class="bottom">
        <div class="products secondary">
            <div class="productLinks">
                <button class="productLink current" href="www.budweiser.com">Budweiser</button>
                <button class="productLink" href="www.budweiser.com">Select</button>
                <button class="productLink" href="www.budweiser.com">Select 55</button>
                <button class="productLink" href="www.budweiser.com">Zero</button>
                <button class="productLink" href="www.budweiser.com">Chelada</button>
                <button class="productLink" href="www.budweiser.com">Supreme</button>
            </div>
        </div>
        <div class="veryBottom primary">
            
        </div>
    </div>
    
</section>
