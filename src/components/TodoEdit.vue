<template>
  <div>
    <input v-model="editedText" @keyup.enter="saveEdit" @blur="saveEdit" />
    <button @click="saveEdit">保存</button>
  </div>
</template>

<script>
export default {
  name: 'TodoEdit',
  props: {
    text: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      editedText: this.text
    };
  },
  methods: {
    saveEdit() {
      if (this.editedText.trim() !== '') {
        this.$emit('save', this.editedText);
      }
    }
  }
};
</script>

<style scoped>
button {
  margin-left: 10px;
  padding: 4px 8px;
  background-color: #27ae60;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #2ecc71;
}

input {
  padding: 4px;
}
</style>
