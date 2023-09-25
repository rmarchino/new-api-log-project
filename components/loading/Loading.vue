<template>
    <div v-if="loading" class="loading-overlay">
      <div class="loading-spinner">
        <v-progress-circular indeterminate color="primary"></v-progress-circular>
      </div>
    </div>
</template>
  
<script>
  export default {
    props: {
      loading: Boolean,
    },
  };
</script>
  
<style>
  .loading-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 9999;
  }
  
  .loading-spinner {
    text-align: center;
    color: white;
  }
</style>
  