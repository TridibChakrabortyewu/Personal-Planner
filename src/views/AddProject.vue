<template>
  <form @submit.prevent="handleSubmit">
    <label>Title:</label>
    <input v-model.trim="title" type="text" />
    <label>Details</label>
    <textarea v-model.trim="details"></textarea>
    <pre class="warning">{{ t }} &nbsp;</pre>
    <button>Add Project</button>
  </form>
</template>

<script>
export default {
  data() {
    return {
      title: "",
      details: "",
      msg: "",
      t: "",
      d: "",
    };
  },
  methods: {
    handleSubmit() {
      let project = {
        title: this.title,
        details: this.details,
        complete: false,
      };

      if (project.title && project.details) {
        fetch("https://my-json-server.typicode.com/iamsabbirsobhani/json-server-typicode/projects", {
          method: "POST",
          headers: { "content-type": "application/json" },
          body: JSON.stringify(project),
        }).then(() => this.$router.push({ name: "Home" }));
      } else {
        this.t =
        !project.title && !project.details
          ? `Please Enter 'TITLE' & 'DETAILS'`
          : !project.title
          ? "Please Enter 'TITLE'"
          : "Please Enter 'DETAILS'";
      }
    },
  },
};
</script>

<style>
form {
  background: white;
  padding: 20px;
  border-radius: 10px;
}
label {
  display: block;
  color: #bbb;
  text-transform: uppercase;
  font-size: 14px;
  font-weight: bold;
  letter-spacing: 1px;
  margin: 20px 0 10px 0;
}
input {
  padding: 10px;
  border: 0;
  border-bottom: 1px solid #ddd;
  width: 100%;
  box-sizing: border-box;
}
textarea {
  border: 1px solid #ddd;
  padding: 10px;
  width: 100%;
  box-sizing: border-box;
  height: 100px;
  margin: 0 0 25px 0;
}
form button {
  display: block;
  margin: 20px auto 0;
  background: #00ce89;
  color: white;
  padding: 10px;
  border: 0;
  border-radius: 6px;
  font-size: 16px;
}
.warning{
    color: crimson;
    font-weight: bold;
}

</style>