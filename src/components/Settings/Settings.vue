<template>
  <div class="settings"></div>
</template>

<script>
export default {
  name: "Settings",
  components: {},
  data() {
    return {
      text: "Hello",
    };
  },
  methods: {
    emailInput(e) {
      this.email = e.target.value;
    },
  },
  computed: {
    getAuthorFullName() {
      return `${this.firstName} ${this.secondName} ${this.lastName}`.toUpperCase();
    },
  },
  watch: {
    currentPage(page) {
      this.loadUsers(page);
    },
  },
};
</script>
<style lang="scss"></style>
