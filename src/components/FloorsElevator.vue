<template>

  <div class="elevator"></div>

</template>

<script>

  export default {
    props: {
      goFloor: {
        default: true
      },
      floors: {
        default: true
      },
    },

    data() {
      return {
        thisFloor: 1,
      }
    },

    computed: {
      checkpointFloors() {
        const obj = {}

        this.floors.forEach((item, index) => {
          obj[index + 1] = false
        })

        return obj;
      }
    },

    watch: {
      goFloor() {
        this.setFloorElevator()
      }
    },

    methods: {
      setFloorElevator() {
          const elevator = document.querySelector(".elevator")

          elevator.style.transform = `translateY(${this.goFloor * -100 + 100}px)`
          elevator.style.transition = `all ${this.setTransition()}s linear`

          setTimeout(() => {
            document.querySelectorAll(".floors-button")[this.goFloor-1].removeAttribute("disabled")
            this.thisFloor = this.goFloor
          }, this.setTransition()*1000);
      },

      setTransition() {
        if(this.thisFloor > this.goFloor) {
          return this.thisFloor - this.goFloor
        }

        if(this.goFloor > this.thisFloor) {
          return this.goFloor - this.thisFloor
        }
      }
      // setFloorElevator() {
      //   const elevator = document.querySelector(".elevator")

      //   elevator.style.transform = `translateY(${this.goFloor * -100 + 100}px)`
      //   elevator.style.transition = `all ${this.setTransition()}s linear`

      //   setTimeout(() => {
      //     document.querySelectorAll(".floors-button")[this.goFloor-1].removeAttribute("disabled")
      //     this.thisFloor = this.goFloor
      //     this.setFloorElevator()
      //   }, this.setTransition()*1000);
      // },

      // setTransition() {
      //   if(this.thisFloor > this.goFloor) {
      //     return this.thisFloor - this.goFloor
      //   }

      //   if(this.goFloor > this.thisFloor) {
      //     return this.goFloor - this.thisFloor
      //   }
      // }
    }
  }

</script>

<style>
  
  .elevator {
    width: 100px;
    height: 100px;
    background-color: blue;
    position: absolute;
    left: 0;
    bottom: 0px;
  }

</style>