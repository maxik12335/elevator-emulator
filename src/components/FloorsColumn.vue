<template>
  <div class="floors-column">
    <div 
      class="floors-column__item"
      v-for="floorsColumnItem in floorsColumnItems"
      :key="floorsColumnItem"
    ></div>

    <div class="elevator"></div>
  </div>
</template>

<script>
export default {
  props: {
    floorsColumnItems: {
      default: true
    },
  },

}
</script>

<style>
  .floors-column {
    position: relative;
    margin-right: 20px;
    display: flex;
    flex-direction: column-reverse;
  }

  .floors-column__item {
    width: 100px;
    height: 100px;
    border: 1px solid rgba(128, 128, 128, 0.422);
    border-right: 2px solid #757575;
    border-left: 2px solid #757575;
  }

  .elevator {
    width: 100px;
    height: 100px;
    position: absolute;
    left: 0;
    bottom: 0;
    background-color: #262bf947;
  }
</style>