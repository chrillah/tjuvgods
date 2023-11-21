<script setup></script>

<template>
  <div class="hero">
    <h1 class="big-title">Som hittat</h1>
  </div>
  <div class="filter-sort-wrapper">
    <div class="filter-sort-container">
      <div class="search-container">
        <input
          @input="this.products = searchProducts(this.searchResult)"
          placeholder="Sök efter produkt..."
          type="text"
          v-model="searchResult"
        />
      </div>
      <div class="filter-container">
        <select v-model="category" @change="filterCategory(this.category)">
          <option>Allt</option>
          <option>Hittegods</option>
          <option>Kläder</option>
          <option>Skor</option>
          <option>Elektronik</option>
          <option>Glasögon</option>
        </select>
      </div>
    </div>

    <div class="sort-wrapper">
      <div class="sort-container">
        <button class="sort-button" @click="priceLow">Lågt till högt</button>
        <button class="sort-button" @click="priceHigh">Högt till lågt</button>
        <button class="sort-button" @click="alfabeticalHigh">A-Ö</button>
        <button class="sort-button" @click="alfabeticalLow">Ö-A</button>
      </div>
    </div>
  </div>
  <div class="products-wrapper">
    <div class="products-container">
      <ProductItem
        class="product-item"
        v-for="product in products"
        :key="product.id"
        @click="selectProduct(product.id)"
        :image="product.image"
        :price="product.price"
        :title="product.title"
      />
    </div>
  </div>
</template>

<style scoped>
*::before,
*::after {
  box-sizing: border-box;
}
p,
h1,
select,
button {
  margin: 0;
  padding: 0;
}
.hero {
  min-height: 500px;
  background-color: var(--primary-red);
  display: flex;
  justify-content: center;
  align-items: center;
}

.big-title {
  font-family: "Turret Road", cursive;
  font-weight: bold;
  font-size: 4em;
  margin: 0 auto;
  text-align: center;
  background-color: white;
  padding: 1rem;
}

@media (min-width: 768px) {
  .big-title {
    font-size: 6em;
  }
}

@media (min-width: 1024px) {
  .big-title {
    font-size: 8em;
  }
}
.search-container input,
.filter-container select,
.sort-button {
  font-size: 0.8rem;
}

.filter-sort-wrapper {
  margin: 0.4rem;
  display: grid;
  gap: 0.4rem;
}

@media (min-width: 485px) {
  .filter-sort-wrapper {
    grid-template-columns: 1fr 1fr;
  }
}

@media (min-width: 629px) {
  .filter-sort-wrapper {
    display: flex;
    justify-content: space-between;
  }
}

.search-container {
  display: flex;
}
.search-container input {
  width: 100%;
  border: 1px solid #000000;
}
.filter-sort-wrapper input:focus {
  border: 2px solid transparent;
  outline: 2px solid var(--primary-red);
}

.filter-sort-container {
  gap: 0.3rem;
  display: grid;
}

.filter-container {
  display: grid;
}

.filter-container select {
  width: 100%;
  border: 1px solid #000000;
  cursor: pointer;
}

.sort-container {
  gap: 0.3rem;
  display: grid;
  grid-template-columns: 1fr 1fr;
}
.sort-button {
  font-weight: bolder;
  padding: 0 1rem;
  border: 1px solid #000000;
}

.search-container input:hover,
.filter-container select:hover,
.sort-button:active,
.sort-button:hover {
  border: 1px solid var(--primary-red);
}

@media (min-width: 800px) {
  .filter-container {
    /* position: relative;
    display: flex; */
  }

  .sort-container {
    /* gap: 0.3rem;
    display: flex; */
  }
}

.product-item {
  cursor: pointer;
}

.products-wrapper {
  background-color: white;
  padding-top: 1rem;
  margin: 0 .4rem;
}

.products-container {
  position: relative;
  display: grid;
  gap:.6rem;
  grid-template-columns: 1fr;
}

@media (min-width: 375px) {
  .products-container {
    grid-template-columns: 1fr 1fr;
  }
}

@media (min-width: 550px) {
  .products-container {
    grid-template-columns: 1fr 1fr 1fr;
  }
}

@media (min-width: 768px) {
  .products-container {
    grid-template-columns: 1fr 1fr 1fr 1fr;
  }
}

@media (min-width: 1024px) {
  .products-container {
    grid-template-columns: 1fr 1fr 1fr 1fr 1fr;
  }
}
</style>

<script>
import ProductItem from "../components/ProductItem.vue";
import axios from "axios";
export default {
  components: {
    ProductItem,
  },
  mounted() {
    this.fetchProducts();
  },
  methods: {
    priceHigh() {
      this.products.sort((a, b) => b.price - a.price);
    },
    alfabeticalHigh() {
      this.products.sort((a, b) => a.title.localeCompare(b.title));
    },
    alfabeticalLow() {
      this.products.sort((a, b) => a.title.localeCompare(b.title)).reverse();
    },
    priceLow() {
      this.products.sort((a, b) => a.price - b.price);
    },

    async filterCategory(category) {
      console.log(category);
      if (category === "Allt") {
        await this.fetchProducts();
      } else if (category !== "Allt") {
        await this.fetchProducts();
        this.products = this.products.filter(
          (item) => item.category === category
        );
      }
    },
    searchProducts(search) {
      const matchingProducts = this.leftOverProducts.filter((product) => {
        const title = product.title.toLowerCase();
        return title.includes(search.toLowerCase());
      });
      return matchingProducts;
    },
    mounted() {
      fetch("/productapi.json")
        .then((Response) => Response.json())
        .then((productapi) => {
          this.products = productapi;
        });
    },

    selectProduct(id) {
      this.$router.push({
        name: "productdetail",
        params: { productID: id },
      });
    },
    async fetchProducts() {
      const result = await axios.get("productapi.json", {
        headers: {
          Accept: "application/json",
        },
      });
      this.products = result.data;
      this.leftOverProducts = result.data;
    },
  },
  data() {
    return {
      products: [],
      category: "Allt",
      searchResult: "",
      leftOverProducts: [],
    };
  },
};
</script>
