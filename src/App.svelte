
<script>
    import {Router, Link, Route} from "svelte-routing"
    import Header from "./Components/Header.svelte";
    import ProductList from "./components/Products/ProductList.svelte";
    import ProductModal from "./components/Products/ProductModal.svelte";
    import {selectedCategory, searchQuery, sortOption} from "./store/store";
    import {onMount} from "svelte";
    import ProductDetail from "./components/Products/ProductDetail.svelte";

    export let url = "";

  onMount(() => {
    const params = new URLSearchParams(window.location.search);
    selectedCategory.set(params.get('category') || '');
    searchQuery.set(params.get('search') || '');
    sortOption.set(params.get('sort') || 'default');
  });
</script>

<style>

</style>

<Router {url}>
    <Header />
    <ProductList/>

    <main>
        <Route path="/" component={ProductList} />
        <Route path="/product/:id" component={ProductModal} />
    </main>
</Router>