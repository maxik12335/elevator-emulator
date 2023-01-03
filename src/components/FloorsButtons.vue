<template>
  <div class="floors-buttons">

    <div 
      v-for="floor in floors"
      :key="floor"
      class="floor-buttons__item"
    >
      <button 
        class="floors-button"
        @click="$emit('floorClick', floor)"
      >
        <div class="in-button-color"></div>
        <div class="in-button-border"></div>
        {{ floor }}
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

  .floors-button {
    background-color: transparent;
    border: 1px solid black;
    border-radius: 26%;
    cursor: pointer;
    position: relative;
    width: 60px;
    height: 60px;
  }

  .floors-button:hover {
    background-color: #f4eeee;
  }

  .floors-button:hover .in-button-color {
    border-color: blue
  }

  .in-button-color {
    position: absolute;
    left: 1px;
    top: 1px;
    width: 56px;
    height: 56px;
    border: 2px solid transparent;
    border-radius: 26%;
  }

  .in-button-border {
    position: absolute;
    left: 4px;
    top: 4px;
    width: 50px;
    height: 50px;
    border: 1px solid black;
    border-radius: 26%;
  }
</style>