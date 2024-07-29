<script>
    import { onMount } from 'svelte';
    import { getCategories, fetchSingleProduct } from './api'; 
  
    export let id;
    let product = {};
    let error = null;
    let categories = [];
  
    onMount(async () => {
      try {
        const productData = await fetchSingleProduct(id);
        if (productData.error) {
          throw productData.error;
        }
        product = productData.response;
  
        const categoryData = await getCategories();
        if (categoryData.error) {
          throw categoryData.error;
        }
        categories = categoryData.response;
      } catch (err) {
        error = err.message;
      }
    });
  </script>
  
  {#if error}
    <p>{error}</p>
  {:else if !product || Object.keys(product).length === 0}
    <p>Loading...</p>
  {:else}
    <div class="mt-6 sm:mt-8 lg:flex lg:items-start lg:max-w-6xl xl:max-w-7xl">
      <div class="mx-auto w-2/5 flex-none">
        <img src={product.image} alt="" class="w-[90%] h-[90%]" />
      </div>
      <div class="mx-auto w-[90%] space-y-2">
        <h1 class="text-2xl md:text-4xl lg:text-4xl font-bold">{product.title}</h1>
        {#if product.rating}
          <div class="flex gap-2">
            <svg class="w-4 h-4 text-yellow-300 ms-1" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="currentColor" viewBox="0 0 22 20">
              <path d="M20.924 7.625a1.523 1.523 0 0 0-1.238-1.044l-5.051-.734-2.259-4.577a1.534 1.534 0 0 0-2.752 0L7.365 5.847l-5.051.734A1.535 1.535 0 0 0 1.463 9.2l3.656 3.563-.863 5.031a1.532 1.532 0 0 0 2.226 1.616L11 17.033l4.518 2.375a1.534 1.534 0 0 0 2.226-1.617l-.863-5.03L20.537 9.2a1.523 1.523 0 0 0 .387-1.575Z" />
            </svg>
            <div>{product.rating.rate}</div>
            <div>Reviews: {product.rating.count}</div>
          </div>
        {/if}
        <span
          key={product.category}
          class="inline-flex items-center rounded-md bg-gray-50 px-2 py-1 text-xs font-medium text-gray-600 ring-1 ring-inset ring-gray-500/10"
        >
          {product.category}
        </span>
        <h3 class="text-xl md:text-2xl lg:text-2xl font-bold">${product.price}</h3>
        <button class="bg-cyan-700 hover:bg-cyan-900 w-[90%] md:w-[14rem] lg:w-[14rem] text-white font-bold py-2 px-4 rounded">
          Add To Cart
        </button>
        <h2 class="text-lg font-bold">Description</h2>
        <p>{product.description}</p>
      </div>
    </div>
  {/if}
  
  <style>
    .mt-6 {
      margin-top: 1.5rem;
    }
    .sm\:mt-8 {
      margin-top: 2rem;
    }
    .lg\:flex {
      display: flex;
    }
    .lg\:items-start {
      align-items: flex-start;
    }
    .lg\:max-w-6xl {
      max-width: 72rem;
    }
    .xl\:max-w-7xl {
      max-width: 80rem;
    }
    .mx-auto {
      margin-left: auto;
      margin-right: auto;
    }
    .w-2\/5 {
      width: 40%;
    }
    .flex-none {
      flex: none;
    }
    .w-\[90\%] {
      width: 90%;
    }
    .h-\[90\%] {
      height: 90%;
    }
    .space-y-2 > :not([hidden]) ~ :not([hidden]) {
      --tw-space-y-reverse: 0;
      margin-top: calc(0.5rem * calc(1 - var(--tw-space-y-reverse)));
      margin-bottom: calc(0.5rem * var(--tw-space-y-reverse));
    }
    .text-2xl {
      font-size: 1.5rem;
      line-height: 2rem;
    }
    .md\:text-4xl {
      font-size: 2.25rem;
      line-height: 2.5rem;
    }
    .lg\:text-4xl {
      font-size: 2.25rem;
      line-height: 2.5rem;
    }
    .font-bold {
      font-weight: 700;
    }
    .flex {
      display: flex;
    }
    .gap-2 > :not([hidden]) ~ :not([hidden]) {
      --tw-space-x-reverse: 0;
      margin-right: calc(0.5rem * var(--tw-space-x-reverse));
      margin-left: calc(0.5rem * calc(1 - var(--tw-space-x-reverse)));
    }
    .w-4 {
      width: 1rem;
    }
    .h-4 {
      height: 1rem;
    }
    .text-yellow-300 {
      --tw-text-opacity: 1;
      color: rgba(253, 230, 138, var(--tw-text-opacity));
    }
    .ms-1 {
      margin-inline-start: 0.25rem;
    }
    .inline-flex {
      display: inline-flex;
    }
    .items-center {
      align-items: center;
    }
    .rounded-md {
      border-radius: 0.375rem;
    }
    .bg-gray-50 {
      --tw-bg-opacity: 1;
      background-color: rgba(249, 250, 251, var(--tw-bg-opacity));
    }
    .px-2 {
      padding-left: 0.5rem;
      padding-right: 0.5rem;
    }
    .py-1 {
      padding-top: 0.25rem;
      padding-bottom: 0.25rem;
    }
    .text-xs {
      font-size: 0.75rem;
      line-height: 1rem;
    }
    .font-medium {
      font-weight: 500;
    }
    .text-gray-600 {
      --tw-text-opacity: 1;
      color: rgba(75, 85, 99, var(--tw-text-opacity));
    }
    .ring-1 {
      box-shadow: 0 0 0 calc(1px + var(--tw-ring-offset-width)) var(--tw-ring-color);
    }
    .ring-inset {
      box-shadow: inset 0 0 0 calc(1px + var(--tw-ring-offset-width)) var(--tw-ring-color);
    }
    .ring-gray-500\/10 {
      --tw-ring-opacity: 1;
      --tw-ring-color: rgba(107, 114, 128, 0.1);
    }
    .text-xl {
      font-size: 1.25rem;
      line-height: 1.75rem;
    }
    .md\:text-2xl {
      font-size: 1.5rem;
      line-height: 2rem;
    }
    .lg\:text-2xl {
      font-size: 1.5rem;
      line-height: 2rem;
    }
    .bg-cyan-700 {
      --tw-bg-opacity: 1;
      background-color: rgba(8, 145, 178, var(--tw-bg-opacity));
    }
    .hover\:bg-cyan-900:hover {
      --tw-bg-opacity: 1;
      background-color: rgba(2, 132, 199, var(--tw-bg-opacity));
    }
    .w-\[14rem] {
      width: 14rem;
    }
    .text-white {
      --tw-text-opacity: 1;
      color: rgba(255, 255, 255, var(--tw-text-opacity));
    }
    .py-2 {
      padding-top: 0.5rem;
      padding-bottom: 0.5rem;
    }
    .px-4 {
      padding-left: 1rem;
      padding-right: 1rem;
    }
    .rounded {
      border-radius: 0.25rem;
    }
    .text-lg {
      font-size: 1.125rem;
      line-height: 1.75rem;
    }
  </style>
  