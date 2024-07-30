

<script>
    import { onMount } from 'svelte';
    import { Link } from "svelte-routing";

    import { selectedCategory, searchQuery, sortOption } from "../store/store"

    export let id;
    let product = null;
    let isLoading = true;

    onMount(async () => {
      try {
        const res = await fetch(`https://fakestoreapi.com/products/${id}`);
        if (!res.ok) throw new Error('Failed to fetch product');
        product = await res.json();
      } catch (error) {
        console.error('Error fetching product:', error);
      } finally {
        isLoading = false;
      }
    });
  
    $: backUrl = `/?category=${$selectedCategory}&search=${$searchQuery}&sort=${$sortOption}`;
</script>

{#if isLoading}
    <div class="loading-spinner">
      <div class="spinner"></div>
    </div>
  {:else if product}
    <div class="modal-content">
      <img src={product.image} alt={product.title} />
      <h2>{product.title}</h2>
      <div class="product-details">
        <p class=""><strong>Price:</strong> ${product.price.toFixed(2)}</p>
        <p><strong>Category:</strong> {product.category}</p>
        <p><strong>Rating:</strong> {product.rating.rate} ★ ({product.rating.count} reviews)</p>
        <p><strong>Description:</strong> {product.description}</p>
      </div>
      <button class="bg-blue-500 text-white px-4 py-2 rounded mt-3 hover:bg-pink-200">Add to favourates</button>
      <Link to={backUrl} class="bg-blue-500 text-white px-4 py-2 rounded mt-3 hover:bg-blue-600">Back to Results</Link>
    </div>
  {:else}
    <p>Error loading product. Please try again.</p>
  {/if}