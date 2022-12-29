<template>
  <div class="container">
    
    <div class="floors-container">
      <div class="floors-column">
        <div class="floors-column__item"></div>
        <div class="floors-column__item"></div>
        <div class="floors-column__item"></div>
        <div class="floors-column__item"></div>
        <div class="floors-column__item"></div>
      </div>

      <div class=" ">
        <div class="floors-column__item"></div>
        <div class="floors-column__item"></div>
        <div class="floors-column__item"></div>
        <div class="floors-column__item"></div>
        <div class="floors-column__item"></div>
      </div>

      <div class="floors-column">
        <div class="floors-column__item"></div>
        <div class="floors-column__item"></div>
        <div class="floors-column__item"></div>
        <div class="floors-column__item"></div>
        <div class="floors-column__item"></div>
      </div>

      <div class="floors-column">
        <div class="floors-column__item"></div>
        <div class="floors-column__item"></div>
        <div class="floors-column__item"></div>
        <div class="floors-column__item"></div>
        <div class="floors-column__item"></div>
      </div>

      <div class="floors-buttons">
        <div class="floor-buttons__item">
          <p class="floors-button__number">1 этаж</p>
          <button class="floors-button">Вызвать лифт</button>
        </div>

        <div class="floor-buttons__item">
          <p class="floors-button__number">2 этаж</p>
          <button class="floors-button">Вызвать лифт</button>
        </div>

        <div class="floor-buttons__item">
          <p class="floors-button__number">3 этаж</p>
          <button class="floors-button">Вызвать лифт</button>
        </div>

        <div class="floor-buttons__item">
          <p class="floors-button__number">4 этаж</p>
          <button class="floors-button">Вызвать лифт</button>
        </div>

        <div class="floor-buttons__item">
          <p class="floors-button__number">5 этаж</p>
          <button class="floors-button">Вызвать лифт</button>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
export default {

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

  .floors-column {
    margin-right: 20px;
  }

  .floors-column__item {
    width: 100px;
    height: 100px;
    border: 1px solid rgba(128, 128, 128, 0.422);
    border-right: 2px solid #757575;
    border-left: 2px solid #757575;
  }

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