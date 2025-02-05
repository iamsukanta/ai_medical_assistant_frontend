<script setup>
  import Loading from 'vue-loading-overlay';
  import 'vue-loading-overlay/dist/css/index.css';
  defineProps({
    loadingValue: {
      type: Boolean,
      required: true,
      default: false
    },
    isFullPage: {
      type: Boolean,
      required: false,
      default: true
    }
  });
</script>

<template>
  <div>
    <Loading
      :active="loadingValue"
      :can-cancel="true"
      :is-full-page="isFullPage"
      :color="'#00C7EF'"
      :width="40"
      :height="40"
    />
  </div>
</template>