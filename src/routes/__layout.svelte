
<script>
    import '../styles/global.css'; 
    import { onMount } from 'svelte';

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

    });



</script>

<div id="main">
    <div class="brands">
        <div class="brand">
            <img class="brandLogo" src="./img/logos/our-beers/busch.png" alt="Budlights Logo"  />
         </div>
         <div class="brand">
            <img class="brandLogo" src="./img/logos/our-beers/busch.png" alt="Budlights Logo"  />
         </div>
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
        
    <slot>
                
    </slot>

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
    
</div>