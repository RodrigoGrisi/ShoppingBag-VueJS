<template>
  <div class="basket">
    <div class="items">
      <div class="item" v-for="(p, idx) in productsInBag" :key="idx">
        <div class="photo">
          <img :src="p.image" alt="foto do produto" />
        </div>
        <div class="description">
          <h4>{{ p.title }}</h4>
          <p class="truncate">{{ p.description }}</p>
        </div>
        <div class="quantityProduct">
          <button class="quantity">+</button>
          <div>Quantiade: {{ p.quantity }}</div>
          <button class="quantity">-</button>
          <div class="price">
            <div class="remove" @click="removeFromBag(p.id)">Remove Item</div>
            R$ <span class="amount">{{ (p.price * 5).toFixed(2) }}</span>
          </div>
        </div>
      </div>
    </div>
    <div class="grand-total">
      Total: R$ {{ grandTotal.toFixed(2) }}
      <button class="buttonPay">Pagar</button>
    </div>
  </div>
</template>

<script>
import { mapState } from 'vuex'

export default {
  computed: {
    ...mapState({
      productsInBag: (state) => state.productsInBag
    }),
    grandTotal() {
      return this.productsInBag.reduce((total, product) => total + product.price * 5, 0)
    }
  },
  methods: {
    removeFromBag(productId) {
      this.$store.dispatch('removeFromBag', productId)
    }
  }
}
</script>

<style lang="scss">
.basket {
  padding: 20px 0;
  display: flex;
  .items {
    min-width: 800px;
    margin: auto;
    border: 1px solid black;
    padding: 50px;
    border-radius: 8px;
    margin-right: 10px;
    .item {
      display: flex;
      justify-content: space-between;
      padding: 40px 0;
      border-bottom: 1px solid lightgrey;
      position: relative;

      .remove {
        position: absolute;
        top: 8px;
        right: 0;
        font-size: 11px;
        text-decoration: underline;
        cursor: pointer;
      }

      .photo {
        img {
          max-width: 80px;
        }
      }

      .description {
        padding-left: 30px;
        box-sizing: border-box;
        max-width: 50%;
      }

      .price {
        .amount {
          font-size: 16px;
          margin-left: 8px;
          vertical-align: middle;
        }
      }
    }

    p.truncate {
      overflow: hidden;
      display: -webkit-box;
      -webkit-line-clamp: 2;
      -webkit-box-orient: vertical;
      white-space: normal;
    }
  }
  .grand-total {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    min-width: 300px;
    background-color: greenyellow;
    max-height: 200px;
    font-size: 24px;
    font-weight: bold;
    text-align: right;
    border-radius: 8px;
  }

  .buttonPay {
    padding: 10px;
    width: 50%;
    margin-top: 20px;
    border: none;
    background-color: black;
    color: white;
    font-size: 16px;
    cursor: pointer;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
    transition: transform 0.2s, box-shadow 0.2s;
  }

  .buttonPay:hover {
    transform: scale(1.05);
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
  }

  .buttonPay:focus {
    outline: none;
  }

  .quantityProduct {
    display: flex;
    justify-content: center;
    align-items: center;
  }

  button.quantity {
    padding: 10px;
    width: 40px;
    margin: 10px;
    border: none;
    background-color: black;
    color: white;
    font-size: 16px;
    cursor: pointer;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
    transition: transform 0.2s, box-shadow 0.2s;
  }

  button.quantity:hover {
    transform: scale(1.1);
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
  }

  button.quantity:focus {
    outline: none;
  }
}
</style>
