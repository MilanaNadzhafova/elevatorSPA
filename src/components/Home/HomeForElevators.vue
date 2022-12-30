<template>
  <div class="home">
    <div class="container">
      <div class="home__wrap home__house">
        <!-- <span class="home__triangle"></span> -->

        <div
          v-for="(floor, index) in floors"
          :key="floor"
          :style="{ height: heightRow() }"
          class="home__floor"
        >
          <btn-floors
            @callElevator="callElevator"
            @floorActive="floorActive"
            :addIsActive="addIsActive"
            :removeIsActive="removeIsActive"
            :floor="index"
          ></btn-floors>
        </div>
        <elevator-cabine
          @addActive="addActive"
          @removeActive="removeActive"
          :elevators="elevators"
          :floors="floors"
          :floor="floor"
        />
      </div>
    </div>
  </div>
</template>

<script>
import ElevatorCabine from "../Elevator/ElevatorCabine.vue";
import BtnFloors from "../BtnFloors/BtnFloors.vue";

export default {
  name: "HomeForElevators",
  components: { ElevatorCabine, BtnFloors },
  data() {
    return {
      floors: 5,
      floor: 1,
      elevators: [],
      removeIsActive: null,
      addIsActive: null,
    };
  },
  methods: {
    // высота строчки этажа
    heightRow() {
      return 100 / this.floors + "%";
    },

    // вызов лифта
    callElevator(floor) {
      this.elevators[0].callElevator(floor);
    },

    //для активности кнопок

    floorActive(floor) {
      this.floor = floor;
    },
    addActive(floor) {
      this.addIsActive = floor;
    },
    removeActive(floor) {
      this.removeIsActive = floor;
    },
  },
};
</script>
<style lang="scss">
@import "./styles/home.scss";
</style>
