<script lang="ts">
    import { onMount } from 'svelte';

    let isMobile:boolean; 

    export let product: string = '';
    export let productCompany: string = '';
    export let productDescription: string = '';
    export let productSrc: string = '';
    export let productPrice: string = '';
    export let productOriginalPrice: string = '';
    export let productSalePercent: string = '';

    function checkMobile():void {
       isMobile = window.matchMedia('only screen and (max-width: 768px)').matches;
    }

    async function fetchProductData():Promise<any> {
        return await fetch('/products.json')
        .then(function(response) {
            return response.json();
        })
        .then(data => {
            return data;
        });
    }

    async function fetchUserData():Promise<any> {
        return await fetch('/users.json')
        .then(function(response) {
            return response.json();
        })
        .then(data => {
            return data;
        });
    }

    function setProductData(products:any) {
        // console.log(JSON.stringify(products.products, null, 4));
        let sneakers = products.products[0];
        product = sneakers.product;
        productCompany = sneakers.product_company;
        productDescription = sneakers.product_description;
        productSrc = '/images/' + sneakers.product_image_sources.first;
        productPrice = sneakers.product_price;
        productOriginalPrice = sneakers.product_original_price;
        productSalePercent = sneakers.product_sale_percentage;
    } 

    function setUserData(users:any) {
        console.log(users);
    }

    onMount(async () => {
        checkMobile();
        const products = await fetchProductData();
        setProductData(products);
        const users = await fetchUserData();
        setUserData(users);
	});
</script>

{#if isMobile}
<div class="mobile-nav-toggle">
    <div class="icon menu"></div>
    <div class="icon close"></div>
</div>

<img src="/images/logo.svg" alt="sneakers logo" />

<nav id="mobile-nav-main">
    <ul>
        <li><a href="/">Collections</a></li>
        <li><a href="/men">Men</a></li>
        <li><a href="/women">Women</a></li>
        <li><a href="/about">About</a></li>
        <li><a href="/contact">Contact</a></li>
    </ul>
</nav>

{:else}
<!-- Turn this into a nav component -->
<nav id="desktop-nav-main">
    <img src="/images/logo.svg" alt="sneakers logo" />
    <ul>
        <li><a href="/">Collections</a></li>
        <li><a href="/men">Men</a></li>
        <li><a href="/women">Women</a></li>
        <li><a href="/about">About</a></li>
        <li><a href="/contact">Contact</a></li>
    </ul>
</nav>
{/if}

<!-- This needs to be the product component -->
<div id="product">
    <img src={productSrc} alt="A product" />

    <p>{ productCompany }</p>
    <h1>{ product }</h1>
    <p>{ productDescription }</p>

    <p class="product-price">{ productPrice } <span class="sale-percent">{ productSalePercent }</span></p>
    <p class="product-original-price">{ productOriginalPrice }</p>
    
    <div class="flex-group-row">
        <div class="product-counter flex-group-row">
            <button>-</button>
            <div>0</div>
            <button>+</button>
        </div>
        <button><span class="icon cart"></span>Add to cart</button>
    </div>
</div>

<!-- This needs to go in the footer component -->
<div class="attribution">
    <p>Challenge by <a href="https://www.frontendmentor.io?ref=challenge" rel="noreferrer" target="_blank">Frontend Mentor</a>.</p> 
    <p>Coded by <a href="https://github.com/javiermruelas">Your Name Here</a>.</p>
</div>