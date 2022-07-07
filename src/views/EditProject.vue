<template>
  <h3>Edit Page</h3>
  <form @submit.prevent="handleUpdate">
    <label> Title: </label>
    <input type="text" required v-model="title" />
    <label> Description</label>
    <textarea
      name="description"
      id="desc"
      required
      v-model="details"
    ></textarea>
    <button>Update Project</button>
  </form>
</template>

<script>
export default {
  props: ["id"],
  data() {
    return {
      title: "",
      details: "",
      uri: "http://localhost:3000/projects/" + this.id,
    };
  },
  mounted() {
    fetch(this.uri)
      .then((res) => res.json())
      .then((data) => {
        this.title = data.title;
        this.details = data.details;
      });
  },
  methods: {
    handleUpdate() {
      fetch(this.uri, {
        method: "PATCH",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({
          title: this.title,
          details: this.details,
        }),
      })
        .then(() => {
          this.$router.push("/");
        })
        .catch((err) => console.error(err.message));
    },
  },
};
</script>

<style></style>
