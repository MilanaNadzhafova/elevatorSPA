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
const TIME_MOVING_BETWEEN_FLOOR = 1000; //ms
const TIME_STAND_ON_FLOOR = 1000; //ms

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
    heightElevatorCabine() {
      return 100 / this.floors;
    },

    callElevator(floor) {
      if (!this.queueFloor.includes(floor)) this.queueFloor.push(floor);
      if (this.currentState == elevatorStates.WAIT) this.start();
      this.$emit("addActive", floor);
    },
    async moveUp() {
      return new Promise((r) => {
        setTimeout(r, TIME_MOVING_BETWEEN_FLOOR);
        this.currentFloor++;
        this.currentDirection = movingStates.UP;
        this.information = "↑ " + this.currentFloor;
        this.bottom = this.bottom + this.heightElevatorCabine();
      });
    },
    async moveDown() {
      return new Promise((r) => {
        setTimeout(r, TIME_MOVING_BETWEEN_FLOOR);
        this.currentFloor--;
        this.currentDirection = movingStates.DOWN;
        this.information = "↓ " + this.currentFloor;
        this.bottom = this.bottom - this.heightElevatorCabine();
      });
    },
    async stop() {
      return new Promise((r) => {
        this.currentDirection = movingStates.STAND;
        this.information = this.currentFloor;
        let floorIndex = this.queueFloor.indexOf(this.currentFloor);
        if (floorIndex !== -1) this.queueFloor.splice(floorIndex, 1);
        this.$emit("removeActive", this.currentFloor);
        setTimeout(r, TIME_STAND_ON_FLOOR);
      });
    },
    async start() {
      this.currentState = elevatorStates.MOVING;
      let callFloor;
      while (this.queueFloor.length != 0) {
        callFloor = this.queueFloor[0];
        while (this.currentFloor != callFloor) {
          if (this.queueFloor.includes(this.currentFloor)) {
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
  computed: {},
  watch: {
    currentPage(page) {
      this.loadUsers(page);
    },
  },
  created() {
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
