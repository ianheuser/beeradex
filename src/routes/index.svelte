<script>
    import * as contentful from "contentful"
    import { onMount } from 'svelte';

    let allBrands = [];

    onMount(async() => {

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

<section>

    <div class="brands">

        {#each allBrands as singleBrand }
            <div class="brand">
               {#if singleBrand.fields.logo.fields.file.url }
                    <img class="logo" src={singleBrand.fields.logo.fields.file.url} alt={ "The logo for " + singleBrand.fields.name } />
               
               
                {/if}
            </div>
            
        {/each}

    </div>

</section>

<style>

    .brands {
        display: flex;
        flex-wrap: wrap;
    }

    .brand {
        border: #696969 1px solid;
        max-width: 300px;
        display: flex;
        justify-content: center;
        align-items: center;
        overflow-x: hidden;
        overflow-y: hidden;
        border-radius: 5px;
        margin: 17px;
        object-fit: cover;
    }

    .logo {
        box-sizing: border-box;   
        max-width: 90%;
    }

</style>