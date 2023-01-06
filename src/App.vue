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
      floorsQuentity: 8,

      minesUserEventList: {},
      elevatorStatus: [],
      notFree: -1,

      userEventList: [],
    }
  },

  mounted() {
    // установка стартовых значений
    for(let i = 0; i < this.mineCount; i++) {
      this.elevatorStatus.push(false)
      this.minesUserEventList[`${i}`] = {}

      this.minesUserEventList[`${i}`]["eventList"] = []
      this.minesUserEventList[`${i}`]["translateY"] = 0
      this.minesUserEventList[`${i}`]["floorNumberNow"] = 1
    }
  },


  methods: {
    floorClick(floorNumber) {  
      this.userEventList.push(floorNumber)

      // Последовательность elevators
      let freeElevator = this.elevatorStatus.indexOf(false)

      if(freeElevator === -1) {
        if(this.notFree === this.mineCount - 1) {
          this.notFree = 0
          freeElevator = this.notFree
        } else {
          this.notFree += 1
          freeElevator = this.notFree
        }
      }

      // Добавляю событие каждому отдельному ЛИФТУ
      this.minesUserEventList[freeElevator].eventList.push(floorNumber)

      const lastElementUserEventList = this.userEventList[this.userEventList.length - 1]
      const inButtonColor = document.querySelectorAll(".in-button-color")

      // условия остановки
      if(this.userEventList.length > 1) {
        let res = this.messageElevatorOnFloor(freeElevator, inButtonColor, lastElementUserEventList)
       
        if(res === "stop") {
          return
        }

        let filterArrUserEventList = this.userEventList.filter(item => item === lastElementUserEventList)
        if(filterArrUserEventList.length > 1) {
          console.log("Лифт уже имеет в очереди этот этаж: ", filterArrUserEventList)
          this.userEventList.pop()
          this.minesUserEventList[freeElevator].eventList.pop()
          return
        }

      } else {
        let res = this.messageElevatorOnFloor(freeElevator, inButtonColor, lastElementUserEventList)

        if(res === "stop") {
          return
        }
      }

      // Проверка для  последовательного запуска фукнции translateElevator
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
        const mine = this.minesUserEventList[freeElevator]
      
        arrow.classList.add('arrow-show')

        if(this.checkDirection(floorNumberNow, freeElevator) === "top") {
          elevator.style.transform = `translateY(${mine.translateY -= 100}px)`
          arrow.style.transform = "rotate(180deg)"
          floorNumber.textContent = `${mine.eventList[0]}`
        }

        if(this.checkDirection(floorNumberNow, freeElevator) === "down") {
          elevator.style.transform = `translateY(${mine.translateY += 100}px)`
          arrow.style.transform = "rotate(0deg)"
          floorNumber.textContent = `${mine.eventList[0]}`
        }

        // момент, когда лифт дошёл до этажа
        if(mine.translateY === (mine.eventList[0] - 1)*-100) {
          
          setTimeout(() => {
            arrow.classList.remove('arrow-show')
            elevator.classList.add("elevator-animate")
          }, 1000);

          clearInterval(startElevator)

          setTimeout(() => {
            const inButtonColor = document.querySelectorAll(".in-button-color")
            inButtonColor[mine.eventList[0] - 1].style.borderColor = "transparent"

            elevator.classList.remove("elevator-animate")          

            mine.floorNumberNow = mine.eventList[0]
            mine.eventList.shift()

            this.userEventList.shift()

            // вызвать функции, если есть в очереди этажи
            if(mine.eventList[0]) {
              this.translateElevator(freeElevator, mine.floorNumberNow)
            }
            
            if(!mine.eventList[0]) {
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

    messageElevatorOnFloor(freeElevator, inButtonColor, lastElementUserEventList) {
      inButtonColor[lastElementUserEventList - 1].style.borderColor = "red"

      let lengthObj = Object.keys(this.minesUserEventList).length
      let listFloors = []

      for(let i = 0; i < lengthObj; i ++) {
        listFloors.push(this.minesUserEventList[i].floorNumberNow)
      }

      // проверка на лифт, который только что ушёл, значит этаж свободен
      let indexValue = 0;
      listFloors.forEach((item, index) => {
        if(item === lastElementUserEventList) {
          indexValue = index
          return 
        }
      })

      if(listFloors.includes(lastElementUserEventList) && !this.elevatorStatus[indexValue]) {
        console.log("Вы уже на этом этаже")
        inButtonColor[lastElementUserEventList - 1].style.borderColor = "transparent"
        this.userEventList.pop()
        this.minesUserEventList[freeElevator].eventList.pop()
        return "stop"
      }
    },
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