<template>
  <div class="floors-buttons">

    <div 
      v-for="floor in floors"
      :key="floor"
      class="floor-buttons__item"
    >
      <p class="floors-button__number">{{ floor }} этаж</p>
      <button 
        class="floors-button"
        @click="$emit('floorClick', floor)"
      >
        Вызвать лифт
      </button>

    </div>
  </div>
</template>

<script>
export default {
  props: {
    floors: {
      default: true
    },
  },
}
</script>

<style>
.floors-buttons {
    display: flex;
    flex-direction: column-reverse;
  }

  .floor-buttons__item {
    width: 100px;
    height: 100px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }

  .floors-button__number {
    margin-bottom: 10px;
  }

  .floors-button {
    background-color: transparent;
    border: 1px solid;
    padding: 5px;
    cursor: pointer;
  }

  .floors-button:hover {
    background-color: #f4eeee;
  }
</style>