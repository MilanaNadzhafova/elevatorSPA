<template>
  <div class="btnFloors flex">
    <div
      class="btnFloors__btn"
      @click="callActive"
      :class="{ active: isActive }"
    ></div>
    {{ floor + 1 }}
  </div>
</template>

<script>
export default {
  name: "BtnFloors",
  components: {},
  data() {
    return {
      isActive: false,
    };
  },
  props: {
    floor: {
      type: Number,
      default: 1,
    },
    addIsActive: Number,
    removeIsActive: Number,
  },
  methods: {
    //вызов лифта, передача нужного этажа
    callActive() {
      this.$emit("callElevator", this.floor + 1);
      this.btnFloorActive();
    },
    //активность кнопок (добавление удаление класса)
    btnFloorActive() {
      this.$emit("floorActive", this.floor + 1);
    },
  },
  watch: {
    //добавление удаление класса активности кнопок

    addIsActive() {
      if (this.addIsActive == this.floor + 1) {
        this.isActive = true;
      }
    },
    removeIsActive() {
      if (this.removeIsActive == this.floor + 1) {
        setTimeout(() => {
          this.isActive = false;
        }, 3000);
      }
    },
  },
};
</script>
<style lang="scss">
@import "./styles/btnFloors.scss";
</style>
