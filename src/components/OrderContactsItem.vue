<template>
<li class="dictionary__item">
    <span class="dictionary__key">
      {{ contact.key }}
    </span>
    <span class="dictionary__value">
      {{ contact.value }}
    </span>
  </li>
</template>

<script>
export default {
  props: ['contact'],
};
</script>
