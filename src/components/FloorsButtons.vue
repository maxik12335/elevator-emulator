<template>
  
  <div class="floors-buttons">

    <!-- можно сделать массив, но так меньше кода -->
    <floors-button 
      v-for="floor in floors" 
      :key="floor"
      :numberFloor="floor"
      @clickFloor="clickFloor"
    />

  </div>

</template>

<script>
import FloorsButton from './FloorsButton.vue'

export default {
  components: { FloorsButton },
  props: {
    floors: {
      default: true
    }
  },

  methods: {
    clickFloor(value) {
      this.$emit('clickFloor', value)
    }
  }
}
</script>

<style>

  .floors-buttons {
    display: flex;
    flex-direction: column-reverse;
  }

</style>