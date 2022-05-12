<template></template>

<script>
export default {
  asyncData() {
    $nuxt.$fire.auth.signOut();
    $nuxt.$router.push("/");
  },
};
</script>

<style></style>
