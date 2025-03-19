<template>
  <div>
    <button class="add-button" @click="addNote">Add Note</button>
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
import axios from 'axios';
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
  mounted() {
    this.fetchNotes();
  },
  methods: {
    async fetchNotes() {
      const response = await axios.get('http://localhost:8080/api/notes');
      this.notes = response.data;
    },
    async addNote() {
      const newNote = {
        content: '',
        top: 100,
        left: 100,
      };
      const response = await axios.post('http://localhost:8080/api/notes', newNote);
      this.notes.push(response.data);
    },
    async deleteNote(noteId) {
      await axios.delete(`http://localhost:8080/api/notes/${noteId}`);
      this.notes = this.notes.filter(note => note.id !== noteId);
    },
    async updateNotePosition(id, top, left) {
      const note = this.notes.find(n => n.id === id);
      if (note) {
        note.top = top;
        note.left = left;
        await axios.put(`http://localhost:8080/api/notes/${id}`, note);
      }
    },
    async updateNoteContent(id, content) {
      const note = this.notes.find(n => n.id === id);
      if (note) {
        note.content = content;
        await axios.put(`http://localhost:8080/api/notes/${id}`, note);
      }
    },
  },
};
</script>