<template>
  <div class="new-note">
    <div class="new-note__title">
      <label class="label">
        <div class="label__text">Title</div>
        <input type="text" class="input" v-model="note.title" />
      </label>
    </div>
    <div class="new-note__description">
      <label class="label">
        <div class="label__text">Description</div>
        <textarea
          class="textarea label__textarea"
          v-model="note.descr"
          name=""
          id=""
          cols="30"
          rows="3"
        ></textarea>
      </label>
    </div>
    <div class="new-note__priority">
      <div class="new-note__priority-text">
        Priority:
      </div>
      <select name="priority" v-model="note.priority" id="priority">
        <option
          v-for="(priority, index) in priorities"
          :key="index"
          :value="priority"
        >{{priority}}</option>
      </select>
    </div>
    <div class="new-note__button">
      <button class="btn btnPrimary" @click="addNewNote">New Note</button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    note: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      priorities: ["regular", "important", "very-important"],
    };
  },
  methods: {
    addNewNote() {
      this.$emit("addNewNote");
    },
  },
};
</script>

<style lang="scss">
.new-note__button {
  text-align: center;
}
.new-note__priority {
  display: flex;
  justify-content: center;
  margin-top: 30px;
  margin-bottom: 20px;

  &-text {
    margin-right: 10px;
  }
}
.new-note__button {
  margin-bottom: 30px;
}
.note__close {
  background-color: transparent;
}
</style>
