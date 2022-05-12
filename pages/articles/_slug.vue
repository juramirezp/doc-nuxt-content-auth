<template>
  <div>
    <nuxt-content :document="doc" />
  </div>
</template>

<script>
export default {
  name: "IndexPage",
  async asyncData({ $content, params }) {
    const doc = await $content(
      "articles",
      { deep: true },
      params.slug || "index"
    )
      .fetch()
      .catch((err) => {
        if (err) {
          console.log(err);
          $nuxt.$router.push("/");
        }
      });
    return {
      doc,
    };
  },
};
</script>
