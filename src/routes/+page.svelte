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

    export let userName: string = '';
    export let userProfileSrc: string = '';
    export let userCart: any[] = [];

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
        let sneakers = products[0];
        product = sneakers.product;
        productCompany = sneakers.product_company;
        productDescription = sneakers.product_description;
        productSrc = '/images/' + sneakers.product_image_sources.first;
        productPrice = sneakers.product_price;
        productOriginalPrice = sneakers.product_original_price;
        productSalePercent = sneakers.product_sale_percentage;
    } 

    function setUserData(users:any) {
        let user = users[0];
        userName = user.user_name;
        userProfileSrc = '/images/' + user.user_profile_image_source;
        userCart = user.user_cart;
    }

    onMount(async () => {
        checkMobile();
        const products = await fetchProductData();
        setProductData(products);
        const users = await fetchUserData();
        setUserData(users);
	});

    let itemCounter: number = 0;

    function addItem() {
        itemCounter++;
    }

    function subtractItem() {
        if (itemCounter - 1 < 0) return;

        itemCounter--;
    }

    let sideNav: boolean = false;

    function openSideNav(): void {
        sideNav = true;
    }

    function closeSideNav(): void {
        sideNav = false;
    }

    function openCart():void {
        console.log('todo');
    }
    
    function openProfile():void {
        console.log('todo');
    }

    function scrollLeft(): void {
        console.log('todo');
    }

    function scrollRight(): void {
        console.log('todo');
    }
</script>


{#if isMobile}
<nav id="mobile-nav-main">
    {#if !sideNav}
    <div class="mobile-nav-toggle">
        <button class="icon menu" on:click={openSideNav}></button>
    </div>
    {/if}
    
    <img src="/images/logo.svg" alt="sneakers logo" />

    <button class="nav-icon cart" on:click={openCart}></button>

    <img class="nav-icon" src={userProfileSrc} on:keydown on:click={openProfile} alt="Your profile!" />

    {#if sideNav}
    <div id="sidenav">
        <button class="icon close" on:click="{closeSideNav}"></button>
        <ul>
            <li><a href="/">Collections</a></li>
            <li><a href="/men">Men</a></li>
            <li><a href="/women">Women</a></li>
            <li><a href="/about">About</a></li>
            <li><a href="/contact">Contact</a></li>
        </ul>
    </div>
    {/if}
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
    <div id="image-viewer">
        <button class="left-arrow" on:click={scrollLeft}></button>
        <img src={productSrc} alt="A product" />
        <button class="right-arrow" on:click={scrollRight}></button>
    </div>
    
    <p>{ productCompany }</p>
    <h1>{ product }</h1>
    <p>{ productDescription }</p>

    <p class="product-price">{ productPrice } <span class="sale-percent">{ productSalePercent }</span></p>
    <p class="product-original-price">{ productOriginalPrice }</p>
    
    <div class="flex-group-row">
        <div class="product-counter flex-group-row">
            <button on:click={subtractItem}>-</button>
            <div>{ itemCounter }</div>
            <button on:click={addItem}>+</button>
        </div>
        <button><span class="icon cart"></span>Add to cart</button>
    </div>
</div>

<!-- This needs to go in the footer component -->
<div class="attribution">
    <p>Challenge by <a href="https://www.frontendmentor.io?ref=challenge" rel="noreferrer" target="_blank">Frontend Mentor</a>.</p> 
    <p>Coded by <a href="https://github.com/javiermruelas">Your Name Here</a>.</p>
</div>