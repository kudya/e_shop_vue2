<template>
  <div class="cart__total">
    <p>Доставка: <b>500 ₽</b></p>
    <p>Итого: <b>{{ totalOrderAmount }}</b> товара на сумму <b>{{ orderTotalPrice | numberFormat }} ₽</b></p>
  </div>
</template>

<script>
import numberFormat from '@/helpers/numberFormat';

export default {
  props: ['orderTotalPrice', 'totalOrderAmount'],
  filters: {
    numberFormat,
  },
};
</script>
