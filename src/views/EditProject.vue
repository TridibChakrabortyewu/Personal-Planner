<template>
  <form>
    <label>Title:</label>
    <input v-model="title" type="text" />
    <label>Details</label>
    <textarea v-model="details"></textarea>
    <pre class="warning">{{ t }} &nbsp;</pre>
    <button @click.prevent="onUpdate">Add Project</button>
  </form>

</template>

<script>
export default {
  props: ["id"],
  data() {
    return {
      title: "",
      details: "",
      project: [],
      t: '',
      url: "https://my-json-server.typicode.com/iamsabbirsobhani/json-server-typicode/projects/" + this.id,
    };
  },
  methods: {
    onClick() {
      this.title = this.project.title;
      this.details = this.project.details;
    },
    onUpdate() {
      let p = {
        title: this.title,
        details: this.details,
        complete: false,
      };
      if (p.title && p.details) {
        fetch(this.url, {
          method: "PATCH",
          headers: { "content-type": "application/json" },
          body: JSON.stringify(p),
        }).then(() => this.$router.push({ name: "Home" }));
      } else {
        this.t =
          !p.title && !p.details
            ? `Please Enter 'TITLE' & 'DETAILS'`
            : !p.title
            ? "Please Enter 'TITLE'"
            : "Please Enter 'DETAILS'";
      }
    },
  },
  mounted() {
    fetch(this.url)
      .then((res) => res.json())
      .then((data) => {
        this.title = data.title;
        this.details = data.details;
        this.project = data;
        // this.onClick()
      });
  },
};
</script>

<style>
</style>