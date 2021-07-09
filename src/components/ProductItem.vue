<template>
  <li class="catalog__item">
    <a class="catalog__pic" href="#" @click.prevent="goToPage('product', {id: product.id})">
      <img :src="product.image" :alt="product.title">
    </a>

    <h3 class="catalog__title">
      <a href="#">
        {{ product.title }}
      </a>
    </h3>

    <span class="catalog__price">
      {{ product.price | numberFormat }} ₽
    </span>

    <ul class="colors colors--black">
      <li class="colors__item" v-for="color in product.colors" :key="color">
        <label class="colors__label">
          <input class="colors__radio sr-only" type="radio" :value="color" v-model="currentColor">
          <span class="colors__value" :style="{backgroundColor: color}">
          </span>
        </label>
      </li>
    </ul>
  </li>
</template>

<script>
import eventBus from '@/eventBus';

export default {
  props: ['product'],
  data() {
    return {
      currentColor: null,
    };
  },
  methods: {
    goToPage(pageName, pageParams) {
      eventBus.$emit('goToPage', pageName, pageParams);
    },
  },
};
</script>
