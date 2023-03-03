<template>
  <v-alert
    :type="type"
    text
    outlined
    v-if="isAlert"
    transition="scroll-y-transition"
  >
    <div>{{ msg }}</div>
  </v-alert>
</template>

<script>
export default {
  props: {
    msg: {
      type: String,
      require: true,
    },
    isAlert: {
      type: Boolean,
      default: false,
    },
    type: {
      type: String,
      default: "",
    },
  },
  // mutations: {
  //   isAlert() {
  //     setTimeout(() => {
  //       this.isAlert = false;
  //     }, 3500);
  //   },
  // },
};
</script>

<style></style>
