<script>
  import { onMount } from 'svelte';
  import { writable } from 'svelte/store';
  import { Link } from "svelte-routing";
  import { selectedCategory, searchQuery, sortOption } from '../store/store';
  import Sort from './Sort.svelte';

  const products = writable([]);
  let isLoading = true;

  onMount(async () => {
    try {
      const res = await fetch('https://fakestoreapi.com/products');
      if (!res.ok) throw new Error('Failed to fetch products');
      const data = await res.json();
      products.set(data);
    } catch (error) {
      console.error('Error fetching products:', error);
      products.set([]);
    } finally {
      isLoading = false;
    }
  });

  $: filteredProducts = $products.filter(product => {
    return (!$selectedCategory || product.category === $selectedCategory) &&
           (!$searchQuery || product.title.toLowerCase().includes($searchQuery.toLowerCase()));
  });

  $: sortedProducts = sortProducts(filteredProducts, $sortOption);

  $: categories = [...new Set($products.map(product => product.category))];

  function handleCategoryChange(event) {
    selectedCategory.set(event.target.value);
  }

  function handleSearchChange(event) {
    searchQuery.set(event.target.value);
  }

  function handleSort(option) {
    sortOption.set(option);
  }

  function sortProducts(products, option) {
    let sortedProducts = [...products];
    switch (option) {
      case 'low-to-high':
        return sortedProducts.sort((a, b) => a.price - b.price);
      case 'high-to-low':
        return sortedProducts.sort((a, b) => b.price - a.price);
      default:
        return sortedProducts;
    }
  }
</script>

<style>
  .filters-and-sort {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
    padding: 10px;
    /* background-color: white; */
    border-radius: 8px;
  }

  .filters {
    display: flex;
    align-items: center;
  }

  .filters select,
  .filters input {
    padding: 8px;
    margin-right: 10px;
    border: 1px solid #ddd;
    border-radius: 4px;
    outline: none;
  }

  .filters input {
    flex: 1;
  }

  .product-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
    gap: 20px;
  }

  .product-card {
    background: #ddd;
    border: 1px solid #ddd;
    height: 100%;
    border-radius: 8px;
    padding: 20px;
    text-align: center;
    transition: transform 0.3s, box-shadow 0.3s;
    position: relative;
  }

  .product-card:hover {
    transform: scale(1.05);
    box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
  }

  .product-card img {
    width: 100%;
    height: 20rem;
    margin-bottom: 15px;
    object-fit: contain;
  }

  .product-card h2 {
    font-size: 1.25rem;
    margin-bottom: 10px;
    font-weight: bold;
  }

  .product-card p {
    margin: 5px 0;
  }

  .product-card button,
  .category-button {
    background-color: #2563eb;
    color: black;
    padding: 0.5rem 1rem;
    border-radius: 0.25rem;
    border: none;
    cursor: pointer;
    transition: background-color 0.3s;
  }

  .product-card button:hover,
  .category-button:hover {
    background-color: #1e40af;
  }

  .loading-spinner {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 50vh;
  }

  .spinner {
    border: 4px solid rgba(0, 0, 0, 0.1);
    width: 36px;
    height: 36px;
    border-radius: 50%;
    border-left-color: #09f;
    animation: spin 1s ease infinite;
  }

  @keyframes spin {
    0% {
      transform: rotate(0deg);
    }
    100% {
      transform: rotate(360deg);
    }
  }
</style>

<div class="filters-and-sort">
  <div class="filters">
    <select bind:value={$selectedCategory} on:change={handleCategoryChange}>
      <option value="">All categories</option>
      {#each categories as category}
        <option value={category}>{category}</option>
      {/each}
    </select>
    <input 
      type="text" 
      placeholder="Search products..." 
      bind:value={$searchQuery}
      on:input={handleSearchChange}
    />
  </div>
  <Sort onSort={handleSort} />
</div>

{#if isLoading}
  <div class="loading-spinner">
    <div class="spinner"></div>
  </div>
{:else}
  <div class="product-grid">
    {#each sortedProducts as product (product.id)}
      <Link to={`/product/${product.id}`} >
        <div class="product-card">
          <img src={product.image} alt={product.title} />
        <h2>{product.title}</h2>
        <p>${product.price.toFixed(2)}</p>
        <p>
          <button class="category-button">{product.category}</button>
        </p>
        <p>{product.rating.rate} ★ ({product.rating.count} reviews)</p>
        <button class= "bg-blue-500 text-white px-4 py-2 rounded mt-3 hover:bg-blue-600" >Add to Cart</button>
        </div>
        
      </Link>
    {:else}
      <p>No products found.</p>
    {/each}
  </div>
{/if}
