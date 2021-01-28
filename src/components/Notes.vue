<template>
  <div class="notes" :class="{full: !grid}">
    <div class="notes__header">
      <div class="notes__title title">Notes</div>
      <div class="notes__icons">
        <svg :class="{active: grid}" @click="grid = true" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="8" y1="6" x2="21" y2="6"></line><line x1="8" y1="12" x2="21" y2="12"></line><line x1="8" y1="18" x2="21" y2="18"></line><line x1="3" y1="6" x2="3" y2="6"></line><line x1="3" y1="12" x2="3" y2="12"></line><line x1="3" y1="18" x2="3" y2="18"></line></svg>
        <svg :class="{active: !grid}" @click="grid = false"  xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" ><rect x="3" y="3" width="7" height="7"></rect><rect x="14" y="3" width="7" height="7"></rect><rect x="14" y="14" width="7" height="7"></rect><rect x="3" y="14" width="7" height="7"></rect></svg>
      </div>
    </div>
    <div class="notes__body">
      <div class="notes__list">
        <div class="note"
          v-for="(note, index) in notes"
          :key="index"
          :class="{ 'important': note.priority === 'important', 
                    'very-important': note.priority === 'very-important'}"
          >
          <div class="note__header">
            <h4 class="note__title"
              @click="editTitle(note.id)"
              v-if="!note.editable">
                {{note.title}}
            </h4>
            <input 
              :type="[!note.editable ? 'hidden' : 'text']"
              v-model="titleNote"
              @keyup.enter="submitNewTitleOrDescr(note.id)"
              @keyup.esc="note.editable = false"
              class="note__input"
              >
            
          <button type="button" class="note__close" @click="removeNote(index)">x</button>
          </div>
          <div class="note__body">
            <div 
              class="note__description"
              v-if="!note.editable"
              >{{note.descr}}</div>
            <input 
              :type="[!note.editable ? 'hidden' : 'text']"
              v-model="descrNote"
              @keyup.enter="submitNewTitleOrDescr(note.id)"
              @keyup.esc="note.editable = false"
              class="note__input"
              >
            <div class="note__date">{{note.date}}</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    notes: {
      type: Array,
      required: true
    },
    grid: {
      type: Boolean,
      required: true
    },
    titleNote: {
      type: String
    },
    descrNote: {
      type: String
    }
  },
  methods: {
    removeNote(idx) {
      this.$emit('remove', idx)
    },
    editTitle(id) {
      this.$emit('editTitle', id)
    },
    submitNewTitleOrDescr(id) {
      this.$emit('getNewTitle', id, this.titleNote, this.descrNote)
    },
  }
}
</script>

<style lang="scss">
.notes {
  padding: 40px 0;

  &.full {
    .note {
      width: 100%;
    }
  }
}
.note__input {
margin-right: 30px;
}
.note {
  width: 46%;
  padding: 18px 20px;
  margin-bottom: 20px;
  background-color: #fff;
  transition: width .3s;
}
.note__header,
.notes__header {
  display: flex;
  justify-content: space-between;
}
.notes__header {
  margin-bottom: 10px;
}
.notes__list {
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
}
.note__close {
  cursor: pointer;
  height: 30px;
  width: 30px;
}
.notes__icons {
  svg {
    margin-right: 10px;
    cursor: pointer;

    &:last-child {
      margin-right: 0;
    }
    &.active {
      stroke: #444ce0;
    }
  }
}
.important {
  border: 2px solid yellow;
}
.very-important {
  border: 2px solid red;
}
</style>