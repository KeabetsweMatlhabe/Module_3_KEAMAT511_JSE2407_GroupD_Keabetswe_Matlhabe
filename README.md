# Svelte E-commerce Product Display

This project is a simple e-commerce product display application built using Svelte. It fetches product data from the [Fake Store API](https://fakestoreapi.com/), displays them in a grid, and allows users to filter, search, and sort products. Users can also view detailed information about a product by clicking on it.

## Features

- Fetches products from a remote API
- Displays products in a responsive grid
- Allows filtering by category
- Allows searching by product title
- Allows sorting by price (low-to-high, high-to-low)
- Displays product details including title, image, price, category, rating, and number of reviews
- Add to cart functionality

## Technologies Used

- [Svelte](https://svelte.dev/)
- [Svelte Routing](https://github.com/EmilTholin/svelte-routing)
- [Tailwind CSS](https://tailwindcss.com/)
- [Fake Store API](https://fakestoreapi.com/)

## Component Details
-App.svelte
This is the main component of the application. It imports other components, fetches product data from the API, and manages state for filtering, searching, and sorting products.

-Sort.svelte
This component provides the sorting options for products. It updates the sorting state based on user selection.

-Custom Styles
The project uses Tailwind CSS for styling. Custom styles are defined within Svelte components using the <style> tag.

### Getting Started

- npm install
- npm run dev
- Open your browser and go to http://localhost:5000.

### Setup

1. Clone the repository:
   ```bash
   git clone https://github.com/your-username/svelte-ecommerce-product-display.git
   cd svelte-ecommerce-product-display
