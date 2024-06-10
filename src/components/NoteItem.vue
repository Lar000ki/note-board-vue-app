<template>
  <div
    class="note-item"
    :style="{ top: top + 'px', left: left + 'px' }"
    @mousedown="startDrag"
  >
    <div class="note-background">
      <textarea v-model="content"></textarea>
    </div>
    <button @click="deleteNote">Delete</button>
  </div>
</template>

<script>
export default {
  props: {
    note: Object,
  },
  data() {
    return {
      top: this.note.top,
      left: this.note.left,
      content: this.note.content,
      isDragging: false,
      startX: 0,
      startY: 0,
    };
  },
  watch: {
    content(newContent) {
      this.$emit('update-content', this.note.id, newContent);
    }
  },
  methods: {
    startDrag(e) {
      this.isDragging = true;
      this.startX = e.clientX - this.left;
      this.startY = e.clientY - this.top;
      document.addEventListener('mousemove', this.onDrag);
      document.addEventListener('mouseup', this.stopDrag);
    },
    onDrag(e) {
      if (this.isDragging) {
        this.top = e.clientY - this.startY;
        this.left = e.clientX - this.startX;
        this.$emit('update-position', this.note.id, this.top, this.left);
      }
    },
    stopDrag() {
      this.isDragging = false;
      document.removeEventListener('mousemove', this.onDrag);
      document.removeEventListener('mouseup', this.stopDrag);
    },
    deleteNote() {
      this.$emit('delete-note', this.note.id);
    },
  },
};
</script>

<style scoped>
.note-item {
  position: absolute;
  padding: 10px;
  width: 200px;
  height: 150px;
  cursor: move;
}

.note-background {
  width: 100%;
  height: 100%;
  background-image: url('@/assets/note.png');
  background-size: cover;
  background-position: center;
  position: relative;
}

textarea {
  margin-left: 7px;
  text-align: center;
  width: 89%;
  height: 100%;
  background: transparent;
  border: none;
  resize: none;
  padding: 10px;
  box-sizing: border-box;
  color: #000;
  font-size: 14px;
  outline: none;
}
</style>
