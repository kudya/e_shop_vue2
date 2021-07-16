<template>
  <ul class="cart__orders">
    <OrderCartItem v-for="item in products" :key="item.product.id" :item="item"/>
  </ul>
</template>

<script>
import OrderCartItem from '@/components/OrderCartItem.vue';

export default {
  props: ['products'],
  components: { OrderCartItem },
};
</script>
