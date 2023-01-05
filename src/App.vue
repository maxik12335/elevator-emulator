<template>
  <div class="container">
    
    <div class="floors-container">

      <floorsColumn 
        v-for="min in mineCount"
        :key="min"

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
      mineCount: 3,
      elevatorStatus: [],
      minesUserEventList: {},
      ifMin: -1,
      // listDefaultfloorNumber: [],

      floorsQuentity: 8,
      floorNumberNow: 1,
      translateY: 0,
      // elevatorStatus: false,
      userEventList: [],
    }
  },

  mounted() {
    // let listParams = ["eventList", "translateY"]

    for(let i = 0; i < this.mineCount; i++) {
      this.elevatorStatus.push(false)
      this.minesUserEventList[`${i}`] = {}

      this.minesUserEventList[`${i}`]["eventList"] = []
      this.minesUserEventList[`${i}`]["translateY"] = 0
      this.minesUserEventList[`${i}`]["floorNumberNow"] = 1

      // this.listDefaultfloorNumber.push(1)
    }
  },

  // + 1) список событий (всех): userEventList
  // + 2) список лифтов с их событиями: minesUserEventList: {0: [0,1,2], 1: [3,4,5] ...}
  // 3) св
  // 3) Переписать логику с floorNumber. Он должен работать 
  // внутри функции translateElevator для функции checkDirection

  methods: {
    floorClick(floorNumber) {  
      this.userEventList.push(floorNumber)

      // Какой elevatorStatus (общий) ?
      let freeElevator = this.elevatorStatus.indexOf(false)
      console.log(freeElevator)

      if(freeElevator === -1) {
        if(this.ifMin === this.mineCount - 1) {
          this.ifMin = 0
          freeElevator = this.ifMin
        } else {
          this.ifMin += 1
          freeElevator = this.ifMin
        }
      }

      // Добавляю событие каждому отдельному ЛИФТУ
      this.minesUserEventList[freeElevator].eventList.push(floorNumber)
      // Добавляю floorNumberNow каждому отдельному ЛИФТУ
      // this.minesUserEventList[freeElevator].floorNumberNow = floorNumber


      let lastElementUserEventList = this.userEventList[this.userEventList.length - 1]
      const inButtonColor = document.querySelectorAll(".in-button-color")
      inButtonColor[lastElementUserEventList - 1].style.borderColor = "red"
      if(this.userEventList.length > 1) {
        
        let filterArrUserEventList = this.userEventList.filter(item => item === lastElementUserEventList)
        if(filterArrUserEventList.length > 1) {
          console.log("Лифт уже имеет в очереди этот этаж: ", filterArrUserEventList)
          this.userEventList.pop()
          this.minesUserEventList[freeElevator].eventList.pop()
        }     

      } else {
        
        if(lastElementUserEventList === this.floorNumberNow) {
          console.log("Вы уже на этом этаже")
          inButtonColor[this.userEventList[0] - 1].style.borderColor = "transparent"
          this.userEventList.pop()
          this.minesUserEventList[freeElevator].eventList.pop()
          return
        }

      }

      if(this.elevatorStatus[freeElevator] === true) {
        return
      }

      this.elevatorStatus[freeElevator] = true
      this.translateElevator(freeElevator, this.minesUserEventList[freeElevator].floorNumberNow)
    },

    translateElevator(freeElevator, floorNumberNow) {
      let startElevator = setInterval(() => {
        const elevator = document.querySelectorAll(".elevator")[freeElevator]
        const arrow = document.querySelectorAll(".arrow")[freeElevator]
        const floorNumber = document.querySelectorAll(".floor-number")[freeElevator]
      
        arrow.classList.add('arrow-show')

        if(this.checkDirection(floorNumberNow, freeElevator) === "top") {
          elevator.style.transform = `translateY(${this.minesUserEventList[freeElevator].translateY -= 100}px)`
          arrow.style.transform = "rotate(180deg)"
          floorNumber.textContent = `${this.minesUserEventList[freeElevator].eventList[0]}`
        }

        if(this.checkDirection(floorNumberNow, freeElevator) === "down") {
          elevator.style.transform = `translateY(${this.minesUserEventList[freeElevator].translateY += 100}px)`
          arrow.style.transform = "rotate(0deg)"
          floorNumber.textContent = `${this.minesUserEventList[freeElevator].eventList[0]}`
        }

        // if(this.minesUserEventList[freeElevator].translateY === (this.userEventList[0]-1)*-100) {
        if(this.minesUserEventList[freeElevator].translateY === (this.minesUserEventList[freeElevator].eventList[0] - 1)*-100) {
          
          setTimeout(() => {
            arrow.classList.remove('arrow-show')
            elevator.classList.add("elevator-animate")
          }, 1000);

          clearInterval(startElevator)
          // let floorNumberNow = Number
          if(this.minesUserEventList[freeElevator].eventList[0]) {
            this.minesUserEventList[freeElevator].floorNumberNow = this.minesUserEventList[freeElevator].eventList[0]
          }

          setTimeout(() => {
            const inButtonColor = document.querySelectorAll(".in-button-color")
            inButtonColor[this.minesUserEventList[freeElevator].eventList[0] - 1].style.borderColor = "transparent"

            elevator.classList.remove("elevator-animate")
            this.minesUserEventList[freeElevator].eventList.shift()

            if(this.minesUserEventList[freeElevator].eventList[0]) {
              this.translateElevator(freeElevator, this.minesUserEventList[freeElevator].floorNumberNow)
            }
            
            if(!this.minesUserEventList[freeElevator].eventList[0]) {
              this.elevatorStatus[freeElevator] = false
              console.log(`Лифт №${freeElevator + 1} END`)
            }
          }, 4000);
        
        }
      }, 1000);
    },

    checkDirection(floorNumberNow, freeElevator) {
      if(floorNumberNow > this.minesUserEventList[freeElevator].eventList[0]) {
        return "down"
      }

      if(this.minesUserEventList[freeElevator].eventList[0] > floorNumberNow) {
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