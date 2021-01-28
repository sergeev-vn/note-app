<template>
  <div class="wrapper">
    <div class="wrapper-content">
      <section class="notes-app">
        <div class="container">
          <div class="notes-app__title">
            <h1 class="title">{{title}}</h1>
          </div>
          <message :mess="message" v-if="message"/>
          <newNote 
            :note="note"
            @add-New-Note="addNewNote"
            />
          <search 
            :value="search"
            placeholder="Find notes"
            @search="search = $event"/>
          <notes 
            :notes="notesFilter"
            :grid="grid"
            @remove="removeNote"
            @edit-title="editTitle" 
            @get-new-title="submitNewTitleOrDescr"
            :titleNote="titleNote" 
            :descrNote="descrNote" 
            />
        </div>

      </section>
    </div>
  </div>
</template>

<script>
import message from './components/Message.vue';
import notes from './components/Notes.vue'
import newNote from './components/NewNote.vue'
import search from './components/Search.vue'

export default {
  components: {
    message, notes, newNote, search
  },
  data () {
    return {
      title: 'Notes App',
      titleNote: '',
      descrNote: '',
      message: null,
      search: '',
      grid: true,
      note: {
        id: '',
        title: '',
        descr: '',
        priority: 'regular',
      },
      notes: [
        { id: parseInt(Math.random() * 1000),
          title: 'title 1',
          descr: 'desc1 desc1 desc1 desc1',
          date: new Date(Date.now()).toLocaleString(),
          priority: 'regular'
        },
        { id: parseInt(Math.random() * 1000),
          title: 'title 2',
          descr: 'desc2 desc2 desc2 desc2',
          date: new Date(Date.now()).toLocaleString(),
          priority: 'important'
        },
        { id: parseInt(Math.random() * 1000),
          title: 'title 3',
          descr: 'desc3 desc3 desc3 desc3',
          date: new Date(Date.now()).toLocaleString(),
          priority: 'regular'
        },
        { id: parseInt(Math.random() * 1000),
          title: 'title 4',
          descr: 'desc4 desc4 desc4 desc4',
          date: new Date(Date.now()).toLocaleString(),
          priority: 'very-important'
        }
      ]
    }
  },
  computed: {
    notesFilter () {
      let array = this.notes,
          search = this.search;

      if(!search) return array;

      search = search.trim().toLowerCase()

      array = array.filter(function(el) {

        if (el.title.toLowerCase().indexOf(search) !== -1) {
          return el;
        }
      })

      return array;
    }
  },
  methods: {
    addNewNote() {
      let {title, descr, priority} = this.note;
      if(title === '' | descr === '') {
        return this.message = 'Cannot be empty!'
      }
      this.notes.push({
        id: parseInt(Math.random() * 1000),
        title,
        descr,
        date: new Date(Date.now()).toLocaleString(),
        priority
      })
      this.message = null;
      this.note.title = '';
      this.note.descr = '';
      this.note.priority = 'regular';
    },
    removeNote(idx) {
      this.notes.splice(idx, 1)
    },
    editTitle(id) {
      let array = this.notes
      
      array.map(el => {
        delete el.editable
        if (el.id === id) {
          el.editable = true
          this.titleNote = el.title
          this.descrNote = el.descr
        }
      })
    },
    submitNewTitleOrDescr(id, title, descr) {
      let array = this.notes
      array.map(el => {
        if (el.id === id) {
          el.title = title
          el.descr = descr
          el.date = new Date(Date.now()).toLocaleString()
          delete el.editable
        }
      })
    }
  }
}
</script>

<style>
</style>
