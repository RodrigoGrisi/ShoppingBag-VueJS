<template>
  <div class="home">
    <div class="grid-gallery">
      <div class="grid-single">
        <img
          src="https://images.pexels.com/photos/2586073/pexels-photo-2586073.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1"
        />
      </div>
      <div class="grid-single">
        <img src="https://images.pexels.com/photos/1666779/pexels-photo-1666779.jpeg" />
      </div>
      <div class="grid-single">
        <img
          src="https://images.pexels.com/photos/1680172/pexels-photo-1680172.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1"
        />
      </div>
    </div>
    <div class="products">
      <div
        v-for="(product, index) in products"
        :key="index"
        class="product"
        :class="{ inBag: isInBag(product) }"
      >
        <div class="product-image" :style="{ backgroundImage: 'url(' + product.image + ')' }"></div>
        <h4>{{ product.title }}</h4>
        <div class="upShowprice">
          <p>R$ {{ (product.price * 5).toFixed(2) }}</p>
        </div>
        <button v-if="!isInBag(product)" @click="addToBag(product)">add to bag</button>
        <button v-else class="remove" @click="this.$store.dispatch('removeFromBag', product.id)">
          Remove to Bag
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import { mapState } from 'vuex'

export default {
  name: 'Home',
  computed: {
    ...mapState(['products', 'productsInBag'])
  },
  methods: {
    addToBag(product) {
      product.quantity = 1
      this.$store.dispatch('addToBag', product)
    },
    isInBag(product) {
      return this.productsInBag.some((item) => item.id === product.id)
    }
  },
  mounted() {
    this.$store.dispatch('loadProducts')
  }
}
</script>


<style lang="scss">
.home {
  .products {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;

    .product {
      flex: 0 0 30%;
      box-sizing: border-box;
      box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
      padding: 16px;
      margin: 8px;
      height: 360px;

      @media only screen and (max-width: 769px) {
        flex: 0 0 40%;
      }

      @media only screen and (max-width: 640px) {
        flex: 0 0 90%;
      }

      &.inBag {
        border: 1px solid #007bff;
      }

      .product-image {
        margin: 20px auto;
        width: 160px;
        height: 140px;
        background-size: contain;
        background-position: center;
        background-repeat: no-repeat;
      }
      h4 {
        margin: 22px auto;
        font-size: 12px;
        max-width: 60%;
        font-weight: normal;
      }

      p.price {
        font-size: 20px;
        font-weight: bold;
      }

      button {
        color: #fff;
        background-color: #007bff;
        border: 1px solid #007bff;
        border-radius: 100px;
        font-weight: 400;
        text-align: center;
        padding: 8px 16px;
        cursor: pointer;

        &:hover {
          opacity: 0.8;
        }

        &.remove {
          background-color: transparent;
          border: none;
          color: black;
          text-decoration: underline;
        }
      }
    }
  }
  .grid-gallery {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 50px;

    .grid-single {
      display: flex;
      margin: 2px;
      height: 200px;
      width: 450px;
      height: 180px;
    }

    .grid-single img {
      border-radius: 10px;
      width: 100%;
      object-fit: cover;
    }
  }
}
</style>
