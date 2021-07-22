<template>
  <main class="content container">
    <div class="content__top">
      <ul class="breadcrumbs">
        <li class="breadcrumbs__item">
          <a class="breadcrumbs__link" href="index.html">
            Каталог
          </a>
        </li>
        <li class="breadcrumbs__item">
          <a class="breadcrumbs__link" href="cart.html">
            Корзина
          </a>
        </li>
        <li class="breadcrumbs__item">
          <a class="breadcrumbs__link">
            Оформление заказа
          </a>
        </li>
      </ul>

      <h1 class="content__title">
        Корзина
      </h1>
      <span class="content__info">
        3 товара
      </span>
    </div>

    <section class="cart">
      <form class="cart__form form" action="#" method="POST" @submit.prevent="order">
        <div class="cart__field">
          <div class="cart__data">
            <BaseFormText title="ФИО" :error="formError.name" v-model="formData.name" placeholder="Введите ваше полное имя"/>

            <BaseFormText title="Адрес доставки" :error="formError.address" v-model="formData.address" placeholder="Введите ваш адрес"/>

            <BaseFormText title="Телефон" type="tel" :error="formError.phone" v-model="formData.phone" placeholder="Введите ваш телефон"/>

            <BaseFormText title="Email" type="email" :error="formError.email" v-model="formData.email" placeholder="Введи ваш Email"/>

            <BaseFormTextArea title="Комментарий к заказу" :error="formError.comment" v-model="formData.comment" placeholder="Ваши пожелания" />
          </div>

          <div class="cart__options">
            <h3 class="cart__title">Доставка</h3>
            <ul class="cart__options options">
              <li class="options__item">
                <label class="options__label">
                  <input class="options__radio sr-only" type="radio" name="delivery" value="0" checked="">
                  <span class="options__value">
                    Самовывоз <b>бесплатно</b>
                  </span>
                </label>
              </li>
              <li class="options__item">
                <label class="options__label">
                  <input class="options__radio sr-only" type="radio" name="delivery" value="500">
                  <span class="options__value">
                    Курьером <b>500 ₽</b>
                  </span>
                </label>
              </li>
            </ul>

            <h3 class="cart__title">Оплата</h3>
            <ul class="cart__options options">
              <li class="options__item">
                <label class="options__label">
                  <input class="options__radio sr-only" type="radio" name="pay" value="card">
                  <span class="options__value">
                    Картой при получении
                  </span>
                </label>
              </li>
              <li class="options__item">
                <label class="options__label">
                  <input class="options__radio sr-only" type="radio" name="pay" value="cash">
                  <span class="options__value">
                    Наличными при получении
                  </span>
                </label>
              </li>
            </ul>
          </div>
        </div>

        <OrderCartSummary>
          <button class="cart__button button button--primery" style="display: flex; justify-content: center; align-items: center; max-height: 48px;" type="submit">
            <span v-show="!orderMaking">Оформить заказ</span>
            <BasePreloader v-show="orderMaking" />
          </button>
        </OrderCartSummary>

        <div class="cart__error form__error-block" v-if="formErrorMessage">
          <h4>Заявка не отправлена!</h4>
          <p>
            {{ formErrorMessage }}
          </p>
        </div>
      </form>
    </section>
  </main>
</template>

<script>
import axios from 'axios';

import { API_BASE_URL } from '@/config';
import BaseFormText from '@/components/BaseFormText.vue';
import BaseFormTextArea from '@/components/BaseFormTextArea.vue';
import OrderCartSummary from '@/components/OrderCartSummary.vue';
import BasePreloader from '@/components/BasePreloader.vue';

export default {
  components: {
    BaseFormText, BaseFormTextArea, OrderCartSummary, BasePreloader,
  },
  data() {
    return {
      test: 'TEST',
      formData: {},
      formError: {},
      formErrorMessage: '',
      orderMaking: false,
    };
  },
  methods: {
    order() {
      this.orderMaking = true;
      this.formError = {};
      setTimeout(() => {
        axios.post(`${API_BASE_URL}/api/orders`, {
          ...this.formData,
        }, {
          params: {
            userAccessKey: this.$store.state.userAccessKey,
          },
        })
          .then((response) => {
            this.$store.commit('resetCart');
            this.$store.commit('updateOrderInfo', response.data);
            this.$router.push({ name: 'orderInfo', params: { id: response.data.id } });
            this.orderMaking = false;
          })
          .catch((error) => {
            this.orderMaking = false;
            this.formError = error.response.data.error.request || {};
            this.formErrorMessage = error.response.data.error.message || '';
          });
      }, 2000);
    },
  },

};
</script>
