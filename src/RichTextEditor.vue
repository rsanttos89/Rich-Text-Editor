<template>
  <div :class="['rich-text-editor', editorContainer]">
    <div class="toolbar">
      <button
        title="Negrito"
        :class="[buttonClass, { active: isBoldActive }]"
        @click="toggleStyle('bold')"
      >
        <b>B</b>
      </button>
      <button
        title="Itálico"
        :class="[buttonClass, { active:  isItalicActive }]"
        @click="toggleStyle('italic')"
      >
        <i>I</i>
      </button>
      <button
        title="Sublinhado"
        :class="[buttonClass, { active: isUnderlineActive }]"
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
      isBoldActive: false,
      isItalicActive: false,
      isUnderlineActive: false,
    };
  },
  methods: {
    toggleStyle(style) {
      if (document.execCommand) {
        document.execCommand(style);
      }
      // Atualiza apenas o estado do estilo clicado
      if (style === 'bold') {
        this.isBoldActive = !this.isBoldActive;
      } else if (style === 'italic') {
        this.isItalicActive = !this.isItalicActive;
      } else if (style === 'underline') {
        this.isUnderlineActive = !this.isUnderlineActive;
      }
      this.updateActiveStyles();
      this.$refs.editor.focus();
    },
    updateActiveStyles() {
      const selection = window.getSelection();

      if (selection.rangeCount > 0) {
        const range = selection.getRangeAt(0);
        const parent = range.commonAncestorContainer;

        // Verifica se o elemento pai tem os estilos aplicados
        this.activeStyles.bold = this.hasStyle(parent, 'b') || this.hasStyle(parent, 'strong');
        this.activeStyles.italic = this.hasStyle(parent, 'i') || this.hasStyle(parent, 'em');
        this.activeStyles.underline = this.hasStyle(parent, 'u');
      } else {
        // Se não houver seleção, redefine os estilos como false
        this.activeStyles.bold = false;
        this.activeStyles.italic = false;
        this.activeStyles.underline = false;
      }
    },
    hasStyle(node, tag) {
      // Verifica se o nó ou algum ancestral tem o estilo aplicado
      while (node && node !== this.$refs.editor) {
        if (node.nodeName.toLowerCase() === tag) {
          return true;
        }
        node = node.parentNode;
      }
      return false;
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