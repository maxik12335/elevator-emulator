<template>
  <div class="container">
    
    <div class="floors-container">

      <floorsColumn 
        :floorsColumnItems="floors"
      />

      <floorsButtons 
        :floors="floors"
        @floorClick="floorClick"
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
    floorsColumn,
  },

  data() {
    return {
      floors: 5,
      locationElevator: 1,
      beforeLocationElevator: 1,

      translateY: 0,
      status: false,
      countArr: [],
    }
  },

  methods: {
    floorClick(numberLocationElevator) {  
      this.locationElevator = numberLocationElevator

      this.countArr.push(this.locationElevator)

      if(this.status === true) {
        return
      }

      this.goElevator()
    },

    goElevator() {
      console.log("START")
      this.status = true

      console.log("GO TOP")
      this.goTop() 
    },

    goTop() {
      let test = setInterval(() => {
        const elevator = document.querySelector(".elevator")
        elevator.style.transition = "1s linear"
        
        if(this.check() === "top") {
          elevator.style.transform = `translateY(${this.translateY -= 100}px)`
        }

        if(this.check() === "down") {
          elevator.style.transform = `translateY(${this.translateY += 100}px)`
        }
        

        if(this.translateY === (this.countArr[0]-1)*-100) {
          clearInterval(test)
          if(this.countArr[0]) {
            this.beforeLocationElevator = this.countArr[0]
          }
          this.countArr.shift()
          if(this.countArr[0]) {
            setTimeout(() => {
              this.goTop()
            }, 3000);
          } else {
            setTimeout(() => {
              this.status = false
              console.log("END")
            }, 3000);
          }
        }
      }, 1000);
    },

    check() {
      if(this.beforeLocationElevator > this.countArr[0]) {
        return "down"
      }

      if(this.countArr[0] > this.beforeLocationElevator) {
        return "top"
      }
    },

    setTransition() {
      if(this.beforeLocationElevator > this.locationElevator) {
        return this.beforeLocationElevator - this.locationElevator
      }

      if(this.locationElevator > this.beforeLocationElevator) {
        return this.locationElevator - this.beforeLocationElevator
      }
    }
  },

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