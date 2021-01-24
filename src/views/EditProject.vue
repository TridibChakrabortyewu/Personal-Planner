<template>
  <form>
    <label>Title:</label>
    <input
      @click="onClicks"
      :class="inBorder"
      v-model.trim="title"
      type="text"
    />
    <pre class="warning">{{ g }} &nbsp;</pre>
    <label>Details</label>
    <textarea
      @click="onClicks2"
      :class="{ inBorder: isActivet }"
      v-model.trim="details"
    ></textarea>
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
      t: "",
      g: "",
      isActive: false,
      isActivet: false,
      url:
        "https://my-json-server.typicode.com/iamsabbirsobhani/json-server-typicode/projects/" +
        this.id,
      // url: "https://my-json-server.typicode.com/iamsabbirsobhani/json-server-typicode/projects/" + this.id,
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
    onClicks() {
      this.isActive = true;
      this.isActivet = false;
    },
    onClicks2() {
      this.isActive = false;
      this.isActivet = true;
    },
    onClick() {
      this.title = this.project.title;
      this.details = this.project.details;
    },
    onUpdate() {
      let bellowTh = /^.{1,29}$/g;
      let threeWords = /^([\S]+)\s([\S]+)\s([\S]+)/g;
      let p = {
        title: this.title,
        details: this.details,
        complete: false,
      };
      if (p.title && p.details){
        if (!this.title.match(threeWords)) {
          this.g = `TITLE must contain at least 3 words`;
        } else if (!this.title.match(bellowTh)) {
          this.g = `TITLE can contain only 30 characters`;
        } else {
          this.g = "";

          fetch(this.url, {
            method: "PATCH",
            headers: { "content-type": "application/json" },
            body: JSON.stringify(p),
          }).then(() => this.$router.push({ name: "Home" }));
        }
      }else {
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
      });
  },
};
</script>

<style>
.inBorder {
  border: 3px solid rgba(73, 152, 241, 0.8);
  /* border: 3px solid #0366D6; */
  border-radius: 8px;
}
</style>