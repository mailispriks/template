<template>
  <div>
    <h2 class="mb-3">Tellimused</h2>
    <!-- <p>Count: {{ count }}</p>
    <button @click="addCount()">+</button>
    <button @click="removeCount()">-</button> -->

    <!-- Tellimuse tabel -->
    <b-table striped hover :items="items" :fields="fields">

      <template #cell(price)="data">
        <b class="text-info">{{ data.value }} EUR</b>
      </template>

      <template #cell(actions)="data">
        <b-button v-b-modal.modal-1 variant="success" @click="showProducts(data.item.products, data.item)">Vaata tooteid</b-button>
      </template>
    </b-table>

    

    <!-- Toote tabel -->

    <b-modal id="modal-1" :title="productTableTitle" size="xl">
      <b-table striped hover :items="productItems" :fields="productFields">

        <template #cell(price)="data">
          <b class="text-info">{{ data.value }} EUR</b>
        </template>

      </b-table>
    </b-modal>

  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'Orders',
  data() {
    return {
      count: 0,
      fields: ['orderNumber', 'orderDate', 'status', { key: 'price', label: 'Hind' }, 'actions'],
      items: [],
      productFields: ['productCode', 'name', 'category', 'amount', 'price'],
      productItems: [],
      productTableTitle: 'Pealkiri'
    }
  },
  async created () {
    const orders = await axios({
      url: 'api/orders',
      method: 'GET',
      headers: {}
    })
    console.log('orders', orders)

    this.items = orders.data
  },
  methods: {
    showProducts (products, item) {
      console.log('products', products)
      this.productItems = products
      console.log(item)
      this.productTableTitle = item.orderNumber
    },
    addCount () {
      console.log('Praegune count:', this.count)
      this.count++
    },
    removeCount () {
      console.log('Praegune count:', this.count)
      this.count--
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  h3 {
    margin: 40px 0 0;
  }
  ul {
    list-style-type: none;
    padding: 0;
  }
  li {
    display: inline-block;
    margin: 0 10px;
  }
  a {
    color: #42b983;
  }
</style>
