<template>
  <div class="project" :class="{ complete: project.complete }">
    <div class="actions">
      <h3 @click="showDetails = !showDetails">{{ project.title }}</h3>
      <div class="icons">
        <router-link
          :to="{ name: 'EditProject', params: { id: this.project.id } }"
        >
          <span class="material-icons"> edit </span>
        </router-link>
        <span @click="onDelete" class="material-icons"> delete </span>
        <span @click="onComplete" class="material-icons" :class="{done: this.project.complete}"> done </span>
      </div>
    </div>
    <div v-if="showDetails" class="details">
      <p>{{ project.details }}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: ["project"],
  data() {
    return {
      showDetails: false,
      url: "https://my-json-server.typicode.com/iamsabbirsobhani/json-server-typicode/projects/" + this.project.id,
      // url: "http://localhost:3000/projects/" + this.project.id,
    };
  },
  methods: {
    onDelete() {
      fetch(this.url, { method: "DELETE" })
        .then((res) => this.$emit("delete", this.project.id))
        .catch(new Error("Cant fetch delete..."));
    },
    onComplete() {
      fetch(this.url, {
        method: "PATCH",
        headers: { "content-type": "application/json" },
        body: JSON.stringify({ complete: !this.project.complete }),
      })
        .then(() => this.$emit("complete", this.project.id))
        .catch(new Error("Fetch update error..."));
    },
  },
};
</script>

<style>
.project {
  margin: 20px auto;
  background: white;
  padding: 10px 20px;
  border-radius: 4px;
  box-shadow: 1px 2px 3px rgba(240, 135, 135, 0.05);
  border-left: 4px solid #e90074;
  box-shadow: 1px 1px 10px 0.5px rgb(209, 209, 209);
}
h3 {
  cursor: pointer;
  font-size: 15px;
}
.actions {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.material-icons {
  font-size: 24px;
  margin-left: 10px;
  color: #bbb;
  cursor: pointer;
}
.material-icons:hover {
  color: #777;
}
.image {
  height: 540px;
  width: 540px;
}
.complete {
  border-left: 4px solid #00ce89;
}
.done{
  color: #00ce89;
}
@media (max-width: 425px) {
  .image {
    height: 250px;
    width: 250px;
  }
  .project{
    max-width: 300px;
  }
  .details p{
    font-size: 12px;
    max-width: 280px;
    text-align: left;
  }
}
</style>
