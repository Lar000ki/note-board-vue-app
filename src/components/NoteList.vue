<template>
  <div>
    <button @click="addNote">Add Note</button>
    <note-item
      v-for="note in notes"
      :key="note.id"
      :note="note"
      @delete-note="deleteNote"
      @update-position="updateNotePosition"
      @update-content="updateNoteContent"
    />
  </div>
</template>

<script>
import NoteItem from './NoteItem.vue';

export default {
  components: {
    NoteItem,
  },
  data() {
    return {
      notes: [],
    };
  },
  methods: {
    addNote() {
      const newNote = {
        id: Date.now(),
        content: '',
        top: 100,
        left: 100,
      };
      this.notes.push(newNote);
    },
    deleteNote(noteId) {
      this.notes = this.notes.filter(note => note.id !== noteId);
    },
    updateNotePosition(id, top, left) {
      const note = this.notes.find(note => note.id === id);
      if (note) {
        note.top = top;
        note.left = left;
      }
    },
    updateNoteContent(id, content) {
      const note = this.notes.find(note => note.id === id);
      if (note) {
        note.content = content;
      }
    },
  },
};
</script>

<style scoped>
/* Добавь стили для кнопки и общего контейнера */
</style>
