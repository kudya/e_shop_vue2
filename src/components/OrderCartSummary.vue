<template>
  <div class="cart__block">
    <OrderCartList :products="orderProducts"/>
    <OrderCartTotal :orderTotalPrice="orderTotalPrice" :totalOrderAmount="totalOrderAmount"/>
    <slot/>
  </div>
</template>

<script>
import { mapGetters, mapState } from 'vuex';
import OrderCartList from '@/components/OrderCartList.vue';
import OrderCartTotal from '@/components/OrderCartTotal.vue';

export default {
  components: { OrderCartList, OrderCartTotal },
  computed: {
    ...mapGetters({ orderProductList: 'cartDetailProducts', orderProductsPrice: 'cartTotalPrice' }),
    ...mapState(['orderInfo']),

    orderProducts() {
      return this.orderInfo ? this.orderInfo.basket.items : this.orderProductList;
    },

    totalOrderAmount() {
      return this.orderProducts.reduce((acc, item) => acc + (item.amount ?? item.quantity), 0);
    },

    orderTotalPrice() {
      if (this.orderInfo) {
        return this.orderInfo.basket.items.reduce((acc, item) => acc + (item.quantity * item.price), 0);
      }
      return this.orderProductsPrice;
    },
  },
};
</script>
