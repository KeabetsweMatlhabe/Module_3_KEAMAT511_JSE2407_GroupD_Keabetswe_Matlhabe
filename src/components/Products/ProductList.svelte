<script>
    import { onMount } from 'svelte';
    import { writable } from 'svelte/store';
    import {Link} from "svelte-routing"
    import { selectedCategory, searchQuery, sortOption } from '../.././store/store';

    import Sort from '../Sort.svelte';
  
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
    background-color: #f0f4f8;
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
    background: #fff;
    border: 1px solid #ddd;
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
    max-width: 100%;
    height: auto;
    margin-bottom: 15px;
  }

  .product-card h2 {
    font-size: extrabold;
    margin-bottom: 10px;
  }

  .product-card p {
    margin: 5px 0;
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
        <Link to={`/product/${product.id}`} class="product-card">
          <img src={product.image} alt={product.title} />
          <h2>{product.title}</h2>
          <p>${product.price.toFixed(2)}</p>
          <p>{product.category}</p>
          <p>{product.rating.rate} ★ ({product.rating.count} reviews)</p>
          <button>Add to Cart</button>
        </Link>
      {:else}
        <p>No products found.</p>
      {/each}
    </div>
  {/if}