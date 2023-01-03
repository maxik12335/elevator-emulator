<template>
  <div class="container">
    
    <div class="floors-container">

      <floorsColumn 
        :floorsQuentity="floorsQuentity"
      />

      <floorsButtons 
        :floorsQuentity="floorsQuentity"
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
      floorsQuentity: 5,
      floorNumberNow: 1,
      translateY: 0,
      elevatorStatus: false,
      userEventList: [],
    }
  },

  methods: {
    floorClick(floorNumber) {  
      this.userEventList.push(floorNumber)

      let lastElementUserEventList = this.userEventList[this.userEventList.length - 1]

      const inButtonColor = document.querySelectorAll(".in-button-color")
      inButtonColor[lastElementUserEventList - 1].style.borderColor = "red"

      if(this.userEventList.length > 1) {
        
        let filterArrUserEventList = this.userEventList.filter(item => item === lastElementUserEventList)
        if(filterArrUserEventList.length > 1) {
          console.log("Лифт уже имеет в очереди этот этаж: ", filterArrUserEventList)
          this.userEventList.pop()  
        }     

      } else {
        
        if(lastElementUserEventList === this.floorNumberNow) {
          console.log("Вы уже на этом этаже")
          inButtonColor[this.userEventList[0] - 1].style.borderColor = "transparent"
          this.userEventList.pop()
          return
        }

      }

      if(this.elevatorStatus === true) {
        return
      }

      this.elevatorStatus = true
      this.translateElevator()
    },

    translateElevator() {
      let startElevator = setInterval(() => {
        const elevator = document.querySelector(".elevator")
        const arrow = document.querySelector(".arrow")
        const floorNumber = document.querySelector(".floor-number")
      
        arrow.classList.add('arrow-show')

        if(this.checkDirection() === "top") {
          elevator.style.transform = `translateY(${this.translateY -= 100}px)`
          arrow.style.transform = "rotate(180deg)"
          floorNumber.textContent = `${this.userEventList[0]}`
        }

        if(this.checkDirection() === "down") {
          elevator.style.transform = `translateY(${this.translateY += 100}px)`
          arrow.style.transform = "rotate(0deg)"
          floorNumber.textContent = `${this.userEventList[0]}`
        }

        if(this.translateY === (this.userEventList[0]-1)*-100) {
          
          setTimeout(() => {
            arrow.classList.remove('arrow-show')
            elevator.classList.add("elevator-animate")
          }, 1000);

          clearInterval(startElevator)
          
          if(this.userEventList[0]) {
            this.floorNumberNow = this.userEventList[0]
          }

          setTimeout(() => {
            const inButtonColor = document.querySelectorAll(".in-button-color")
            inButtonColor[this.userEventList[0] - 1].style.borderColor = "transparent"

            elevator.classList.remove("elevator-animate")
            this.userEventList.shift()

            if(this.userEventList[0]) {
              this.translateElevator()
            }
            
            if(!this.userEventList[0]) {
              this.elevatorStatus = false
              console.log("END")
            }
          }, 4000);
        
        }
      }, 1000);
    },

    checkDirection() {
      if(this.floorNumberNow > this.userEventList[0]) {
        return "down"
      }

      if(this.userEventList[0] > this.floorNumberNow) {
        return "top"
      }
    },

    setTransition() {
      let lastElementUserEventList = this.userEventList[this.userEventList.length - 1]

      if(this.floorNumberNow > lastElementUserEventList) {
        return this.floorNumberNow - lastElementUserEventList
      }

      if(lastElementUserEventList > this.floorNumberNow) {
        return lastElementUserEventList - this.floorNumberNow
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