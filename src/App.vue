<template>
  <div id="app">
    <input type="text" v-model="message">
    <ul>
      <li v-for="item in items" :key="item.name">
        {{ item.name }}の個数:
        <input type="number" v-model="item.quantity" min="0">
        <!-- <input type="number"
          v-on:input="item.quantity = $event.target.value"
          v-bind:value="item.quantity"
          min="0"> -->
        <!-- <input type="number"
          @input="test($event)"> -->
      </li>
    </ul>
    <ul>
      <li v-for="item in items" :key="item.name">
        {{ item.name }}: {{ item.price }} x {{ item.quantity }} = {{ item.price * item.quantity | numberWithDelimiter }}円
      </li>
    </ul>
    <p>
      {{ items[0].name }}: {{ items[0].price }} x {{ items[0].quantity }}
    </p>
    <p><button v-bind:disabled="!canBuy" @click="doBuy">購入</button></p>
    <p>小計：{{ totalPrice | numberWithDelimiter }}円</p>
    <p>合計(税込)：{{ totalPriceWithTax | numberWithDelimiter }}円</p>
    <!-- <p v-show="!canBuy">{{ 1000 | numberWithDelimiter }}円以上からご購入いただけます。</p> -->
    <p v-bind:class="errorMessageClass">{{ 1000 | numberWithDelimiter }}円以上からご購入いただけます。</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      message: 'こんにちわ！',
      items: [
        {
          name: '鉛筆',
          price: 300,
          quantity: 0
        },
        {
          name: 'ノート',
          price: 400,
          quantity: 0
        },
        {
          name: '消しゴム',
          price: 500,
          quantity: 0
        },
      ]
    }
  },
  watch: {
    // items: {
    //   handler: function() {
    //     alert('!!')
    //   },
    //   deep: true
    // }
  },
  filters: {
    numberWithDelimiter: function(value) {
      if(!value) {
        return '0'
      }
      return value.toString().replace(/(\d)(?=(\d{3})+$)/g, '$1,')
    }
  },
  computed: {
    totalPrice: function() {
      return this.items.reduce((sum, item) => {
        return sum + (item.price * item.quantity)
      }, 0)
    },
    totalPriceWithTax: function() {
      return Math.floor(this.totalPrice * 1.08)
    },
    canBuy: function() {
      return this.totalPrice >= 1000
    },
    errorMessageClass: function() {
      return { error: !this.canBuy }
    }
  },
  mounted() {

  },
  created() {

  },
  methods: {
    doBuy: function() {
      alert(this.totalPriceWithTax + '円のお買い上げ！')
      this.items.forEach((item) => {
        item.quantity = 0
      })
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.error {
  color: #f00;
}
</style>
