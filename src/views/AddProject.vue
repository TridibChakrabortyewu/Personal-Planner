<template>
  <form @submit.prevent="handleSubmit">
    <label>Title:</label>
    <input
      class="cc"
      @click="onClick"
      :class="inBorder"
      v-model.trim="title"
      type="text"
    />
    <pre class="warning">{{ g }} &nbsp;</pre>
    <label>Details</label>
    <textarea
      @click="onClick2"
      :class="{ inBorder: isActivet }"
      v-model.trim="details"
    ></textarea>
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
      g: "",
      isActive: false,
      isActivet: false,
      // url: "http://localhost:3000/projects",
      url:
        "https://my-json-server.typicode.com/iamsabbirsobhani/json-server-typicode/projects",
    };
  },
  computed: {
    inBorder: function () {
      return {
        inBorder: this.isActive,
      };
    },
  },
  methods: {
    onClick() {
      this.isActive = true;
      this.isActivet = false;
    },
    onClick2() {
      this.isActive = false;
      this.isActivet = true;
    },
    handleSubmit() {
      let project = {
        title: this.title,
        details: this.details,
        complete: false,
      };
      let bellowTh = /^.{1,20}$/g;
      let threeWords = /^([\S]+)\s([\S]+)\s([\S]+)/g;
      if (project.title && project.details) {
        if (!this.title.match(threeWords)) {
          this.g = `TITLE must contains at least 3 words`;
        } else if (!this.title.match(bellowTh)) {
          this.g = `TITLE can contains only 21 characters`;
        } else {
          this.g = "";
          fetch(this.url, {
            method: "POST",
            headers: { "content-type": "application/json" },
            body: JSON.stringify(project),
          }).then(() => this.$router.push({ name: "Home" }));
        }
      } else {
        this.t =
          !project.title && !project.details
            ? `Please fill out 'TITLE' & 'DETAILS' fields`
            : !project.title
            ? "Please fill out 'TITLE' field"
            : "Please fill out 'DETAILS' field";
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
  box-shadow: 1px 1px 10px 0.5px rgb(209, 209, 209);
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
  outline: none;
}
.inBorder {
  border: 3px solid rgba(73, 152, 241, 0.8);
  /* border: 3px solid #0366D6; */
  border-radius: 8px;
}
textarea {
  border: 1px solid #ddd;
  padding: 10px;
  width: 100%;
  box-sizing: border-box;
  height: 100px;
  margin: 0 0 25px 0;
  outline: none;
}
form button {
  display: block;
  margin: 20px auto 0;
  background: #00ce89;
  color: white;
  padding: 10px;
  border: 0;
  border-radius: 6px;
  cursor: pointer;
  font-size: 16px;
}
.warning {
  color: crimson;
  font-weight: bold;
  font-size: 15px;
}

@media (max-width: 425px) {
  .warning {
    font-size: 11px;
  }
}
</style>