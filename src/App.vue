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

      let last = this.countArr[this.countArr.length - 1]

      const inButtonColor = document.querySelectorAll(".in-button-color")
      inButtonColor[last - 1].style.borderColor = "red"

      if(this.countArr.length > 1) {
        
        let filterArr = this.countArr.filter(item => item === last)
        if(filterArr.length > 1) {
          console.log("Лифт уже имеет в очереди этот этаж: ", filterArr)
          this.countArr.pop()  
        }     

      } else {
        
        if(last === this.beforeLocationElevator) {
          console.log("Вы уже на этом этаже")
          inButtonColor[this.countArr[0] - 1].style.borderColor = "transparent"
          this.countArr.pop()
          return
        }

      }

      if(this.status === true) {
        return
      }

      this.goElevator()
    },

    goElevator() {
      this.status = true
      this.goTop() 
    },

    goTop() {
      let test = setInterval(() => {
        const elevator = document.querySelector(".elevator")
        const arrow = document.querySelector(".arrow")
        const floorNumber = document.querySelector(".floor-number")
      
        arrow.classList.add('arrow-show')

        if(elevator.classList.contains("elevator-arrived")) {
          elevator.classList.remove("elevator-arrived")
        }

        elevator.style.transition = "1s linear"

        if(this.check() === "top") {
          elevator.style.transform = `translateY(${this.translateY -= 100}px)`
          arrow.style.transform = "rotate(180deg)"
          floorNumber.textContent = `${this.countArr[0]}`

        }

        if(this.check() === "down") {
          elevator.style.transform = `translateY(${this.translateY += 100}px)`
          arrow.style.transform = "rotate(0deg)"
          floorNumber.textContent = `${this.countArr[0]}`

        }

        if(this.translateY === (this.countArr[0]-1)*-100) {
          setTimeout(() => {
            arrow.classList.remove('arrow-show')
            elevator.classList.add("elevator-arrived")
          }, 1000);
          clearInterval(test)
          if(this.countArr[0]) {
            this.beforeLocationElevator = this.countArr[0]
          }

          setTimeout(() => {
            const inButtonColor = document.querySelectorAll(".in-button-color")
            inButtonColor[this.countArr[0] - 1].style.borderColor = "transparent"
            this.countArr.shift()
            if(this.countArr[0]) {
              this.goTop()
            }
            if(!this.countArr[0]) {
              this.status = false
              console.log("END")
            }
          }, 4000);
        
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