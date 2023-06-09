<template>
  <h1>hehe</h1>
</template>

<script>
export default {
  layout: "empty",
  mounted() {
    this.cekAuth();
  },
  methods: {
    cekAuth() {
      if (!localStorage.getItem("idUser")) {
        this.$router.push("/login");
      } else {
        this.$router.push("/transaction");
      }
    },
  },
};
</script>
