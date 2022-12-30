<template>
    
  <div class="floor-buttons__item">
    <p class="floors-button__number">{{ numberFloor }} этаж</p>
    <button 
      class="floors-button"
      @click="clickButton"
    >
      Вызвать лифт
    </button>
  </div>

</template>

<script>
export default {
  props: {
    numberFloor: {
      default: true
    }
  },

  methods: {
    clickButton(e) {
      this.$emit('clickFloor', this.numberFloor)
      e.target.setAttribute("disabled", "true")
    }
  }
}
</script>

<style>

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