<template>

  <div class="floors-column">
    <floorsColumnItem 
      v-for="floor in floors"
      :key="floor"
    />

    <floorsElevator 
      :goFloor="goFloor"
      :floors="floors"
    />
  </div>

</template>

<script>
  import floorsColumnItem from "@/components/FloorsColumnItem.vue"
  import floorsElevator from '@/components/FloorsElevator.vue'

  export default {
    components: {
      floorsColumnItem,
      floorsElevator
    }, 

    props: {
      floors: {
        default: true
      },
      goFloor: {
        default: 0
      }
    },

  }

</script>

<style>

  .floors-column {
    margin-right: 20px;
    display: flex;
    flex-direction: column-reverse;
    position: relative;
  }
  
  .elevator {
    width: 100px;
    height: 100px;
    background-color: blue;
    position: absolute;
    left: 0;
    bottom: 0px;
  }

</style>