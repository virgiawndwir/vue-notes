<template>
  
  <div class="listNotes">
    <ul>
      <li v-for="(row, index) in notess" :key="index">
        <button class="btn-note" @click="editNote(row.id)">
          <label for="">{{ row.title }}</label>
          <span>{{ row.description }}</span>
        </button>
      </li>
    </ul>
  </div>

</template>

<script>
export default {
  name: 'ListNotes',
  data: function(){
    return {
        notess: [
          {
            id: 1,
            title: 'Judul',
            description: 'Ini isi dari judul'
          },
          {
            id: 2,
            title: 'Baru',
            description: 'Ini isi dari baru'
          }
        ],
      }
  },
  props: {
    propEditNote: {
      type: Function
    }
  },
  methods: {
    editNote(id){
      let dataForm = this.notess.find(note => note.id === id)
      this.$root.$emit('emitForm', dataForm)
    },
    createNewId(){
      let newId = 0
      if(this.notess.length === 0){
        newId = 1
      }else{
        newId = this.notess[this.notess.length - 1].id + 1
      }

      return newId
    }
  },
  mounted(){
    this.$root.$on('emitRemoveNote', data => {
      let noteIndex = this.notess.findIndex(note => note.id === data.id)
      this.notess.splice(noteIndex, 1)
    })

    this.$root.$on('emitUpdateNote', data => {
      let noteIndex = this.notess.findIndex(note => note.id === data.id)
      this.notess[noteIndex].title = data.title
      this.notess[noteIndex].description = data.description
    })

    this.$root.$on('emitSaveNote', data => {
      let newId = this.createNewId()
      let newNote = {
        id : newId,
        'title' : data.title,
        'description' : data.description
      }
      this.notess.push(newNote)
      this.editNote(newId)
    })
  }
}
</script>

<style>
ul {
     list-style-type: none;
     padding: 0;
     margin:0px;
}
.btn-note {
     text-align: left;
     border: none;
     border-bottom: 1px solid gainsboro;
     padding: 0px 15px;
     cursor: pointer;
     outline: none;
     background: #f7f7f7;
     height: 150px;
     width: 100%;
}
.btn-note:hover {
     background:#eaeaea;
}
.btn-note label{
     display:block;
     color: #444444;
     font-size: 1.5em;
     margin-bottom: 15px;
}
.btn-note span{
    color: #545454;
}
</style>