<template>
  <div class="project" :class="{ complete: project.complete }">
    <div class="actions">
      <h3 @click="expand">{{ project.title }}</h3>
      <div class="icons">
        <router-link :to="{ name: 'editproject', params: { id: project.id } }">
          <span class="material-icons"> edit </span>
        </router-link>
        <span @click="deleteItem" class="material-icons"> delete </span>
        <span
          @click="toggleComplete"
          :class="{ completeIcon: project.complete }"
          class="material-icons"
        >
          done
        </span>
      </div>
    </div>
    <transition name="details">
      <div v-if="showDetails" class="details">{{ project.details }}</div>
    </transition>
  </div>
</template>

<script>
export default {
  props: ["project"],
  data() {
    return {
      showDetails: false,
      uri: "http://localhost:3000/projects/" + this.project.id,
    };
  },
  methods: {
    expand() {
      this.showDetails = !this.showDetails;
    },
    deleteItem() {
      fetch(this.uri, { method: "DELETE" })
        .then(() => this.$emit("delete", this.project.id))
        .catch((err) => console.error(err.message));
    },
    toggleComplete() {
      fetch(this.uri, {
        method: "PATCH",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ complete: !this.project.complete }),
      })
        .then(() => {
          this.$emit("complete", this.project.id);
        })
        .catch((err) => console.log(err.message));
    },
  },
};
</script>

<style>
.details-enter-active {
  transition: all 0.4s linear;
}
.details-leave-active {
  transition: unset;
}
.details-enter-from {
  opacity: 0;
}
.details-enter-to {
  opacity: 1;
}

.project {
  margin: 20px auto;
  background: #fff;
  padding: 10px 20px;
  border-radius: 4px;
  box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.05);
  border-left: 4px solid #e90074;
}
.actions {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
h3 {
  cursor: pointer;
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
.complete {
  border-left: 4px solid #00ce89;
}
.completeIcon {
  color: #00ce89;
}
.completeIcon:hover {
  color: #00ce89;
}
</style>
