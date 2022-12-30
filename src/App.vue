<template>
  <div class="container">
    
    <div class="floors-container">

      <floorsColumn 
        :floors="floors" 
        :goFloor="goFloor"
      /> 

      <floorsButtons 
        :floors="floors"
        @clickFloor="clickFloor"
      />

    </div>

  </div>
</template>

<script>
import floorsButtons from "@/components/FloorsButtons.vue"
import floorsColumn from "@/components/FloorsColumn.vue"

export default {
  components: {
    floorsButtons,
    floorsColumn
  },

  data: () => {
    return {
      floors: [1,2,3,4,5],
      goFloor: 0,
    }
  },

  methods: {
    clickFloor(numberFloor) {
      console.log(numberFloor)
      this.goFloor = numberFloor
    },
  }
}
</script>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  .container {
    margin: 0 auto;
    width: 1200px;
  }

  .floors-container {
    display: flex;
    justify-content: flex-start;
    padding: 50px;
  }
</style>