<template>
  <BaseFormField :title="title" :error="error">
    <textarea class="form__input form__input--area" :type="type" v-model="dataValue" :placeholder="placeholder"></textarea>
  </BaseFormField>
</template>

<script>
import formFieldMixin from '@/mixins/formFieldMixin';

export default {
  props: {
    type: {
      default: 'text',
    },
  },
  mixins: [formFieldMixin],
};
</script>
