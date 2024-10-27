<template>
  <div :class="['rich-text-editor', editorContainer]">
    <div class="toolbar">
      <button
        title="Negrito"
        :class="[buttonClass, { active: activeStyles.bold }]"
        @click="toggleStyle('bold')"
      >
        <b>B</b>
      </button>
      <button
        title="Itálico"
        :class="[buttonClass, { active: activeStyles.italic }]"
        @click="toggleStyle('italic')"
      >
        <i>I</i>
      </button>
      <button
        title="Sublinhado"
        :class="[buttonClass, { active: activeStyles.underline }]"
        @click="toggleStyle('underline')"
      >
        <u>U</u>
      </button>
    </div>
    <div
      ref="editor"
      contenteditable="true"
      :class="['editor', editorClass]"
      @input="updateContent"
      @focus="updateActiveStyles"
      @keyup="updateActiveStyles"
    ></div>
  </div>
</template>

<script>
export default {
  name: "RichTextEditor",
  props: {
    content: {
      type: String,
      default: '',
    },
    editorContainer: {
      type: String,
      default: '',
    },
    buttonClass: {
      type: String,
      default: '',
    },
    editorClass: {
      type: String,
      default: '',
    },
  },
  data() {
    return {
      currentContent: this.content,
      activeStyles: {
        bold: false,
        italic: false,
        underline: false,
      },
    };
  },
  methods: {
    toggleStyle(style) {
      document.execCommand(style);
      this.updateActiveStyles();
      this.$refs.editor.focus();
    },
    updateActiveStyles() {
      this.activeStyles.bold = document.queryCommandState("bold");
      this.activeStyles.italic = document.queryCommandState("italic");
      this.activeStyles.underline = document.queryCommandState("underline");
    },
    updateContent() {
      this.currentContent = this.$refs.editor.innerHTML;
      this.$emit("input", this.currentContent);
    },
  },
  mounted() {
    this.$refs.editor.innerHTML = this.content;
    this.$refs.editor.focus();
  },
};
</script>

<style scoped>
.rich-text-editor {
  border: 1px solid #ccc;
  padding: 8px;
}
.toolbar {
  display: flex;
  gap: 8px;
  margin-bottom: 8px;
}
button {
  max-width: 30px;
  min-width: 30px;
  min-height: 30px;
  max-height: 30px;
  border: 1px solid #ccc;
  background-color: white;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  display: flex;
  cursor: pointer;
  font-size: 16px;
  user-select: none;
}
button.active {
  color: white;
  background-color: #007bff;
  border-color: #007bff;
}
.editor {
  outline: none;
  min-height: 200px;
  max-height: 400px;
  border: 1px solid #ccc;
  border-radius: 4px;
  background-color: #f9f9f9;
  padding: 8px;
  overflow-y: auto;
}
</style>