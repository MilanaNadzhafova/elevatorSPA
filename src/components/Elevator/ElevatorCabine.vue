<template>
  <div class="elevator">
    <div
      class="elevator__cabine"
      :style="{ bottom: bottom + '%', height: heightElevatorCabine() + '%' }"
    >
      <div class="elevator__info">{{ information }}</div>
    </div>
  </div>
</template>

<script>
let elevatorStates = { MOVING: 1, WAIT: 2 },
  movingStates = { UP: 1, DOWN: 2, STAND: 3 };
//количество секунд, которое лифт едет между этажами
const TIME_MOVING_BETWEEN_FLOOR = 1000; //ms
//количество секунд, которое лифт стоит на этаже
const TIME_STAND_ON_FLOOR = 3000; //ms

export default {
  name: "ElevatorCabine",
  components: {},
  data() {
    return {
      bottom: 0,
      information: "1",
      queueFloor: [],
      currentFloor: 1,

      currentState: elevatorStates.WAIT,
      currentDirection: movingStates.STAND,
      elevatorsArray: this.elevators,
    };
  },
  props: {
    floor: {
      type: Number,
      default: 1,
    },
    floors: {
      type: Number,
      default: 5,
    },

    elevators: Array,
  },
  methods: {
    // высчитываем высоту кабины лифта
    heightElevatorCabine() {
      return 100 / this.floors;
    },

    // вызов лифта (происходит при нажатии кнопки вызова)
    callElevator(floor) {
      if (!this.queueFloor.includes(floor)) this.queueFloor.push(floor);
      if (this.currentState == elevatorStates.WAIT) this.start();
      this.$emit("addActive", floor);
    },

    // движение лифта вверх
    async moveUp() {
      return new Promise((r) => {
        setTimeout(r, TIME_MOVING_BETWEEN_FLOOR);
        this.currentFloor++;
        this.currentDirection = movingStates.UP;
        this.information = "↑ " + this.currentFloor;
        this.bottom = this.bottom + this.heightElevatorCabine();
      });
    },

    // движение лифта вниз
    async moveDown() {
      return new Promise((r) => {
        setTimeout(r, TIME_MOVING_BETWEEN_FLOOR);
        this.currentFloor--;
        this.currentDirection = movingStates.DOWN;
        this.information = "↓ " + this.currentFloor;
        this.bottom = this.bottom - this.heightElevatorCabine();
      });
    },
    // остановка лифта
    async stop() {
      return new Promise((r) => {
        this.currentDirection = movingStates.STAND;
        this.information = this.currentFloor;
        let floorIndex = this.queueFloor.indexOf(this.currentFloor);
        if (floorIndex !== -1) this.queueFloor.splice(floorIndex, 1);
        setTimeout(r, TIME_STAND_ON_FLOOR);

        this.$emit("removeActive", this.currentFloor);
      });
    },

    // старт лифта
    async start() {
      this.currentState = elevatorStates.MOVING;
      let callFloor;
      while (this.queueFloor.length != 0) {
        callFloor = this.queueFloor[0];
        while (this.currentFloor != callFloor) {
          if (this.queueFloor.includes(this.currentFloor)) {
            this.$emit("addActive", this.currentFloor);

            await this.stop();
          }
          if (this.currentFloor < callFloor) await this.moveUp();
          else await this.moveDown();
        }

        await this.stop();
      }
      this.currentState = elevatorStates.WAIT;
    },
  },
  created() {
    // на будущее если будет несколько лифтов - массив лифтов
    this.elevatorsArray.push(this);
  },
  mounted() {
    this.heightElevatorCabine();
  },
};
</script>
<style lang="scss">
@import "./styles/elevator.scss";
</style>
