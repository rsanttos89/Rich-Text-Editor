```markdown
# RichTextEditor

Um editor de texto rico personalizável construído com Vue.js. Permite que os usuários editem texto com estilos de negrito, itálico e sublinhado. Este componente é projetado para ser flexível e fácil de integrar em suas aplicações Vue.

## Instalação

Você pode instalar o componente `RichTextEditor` diretamente do npm usando o seguinte comando:

```bash
npm install rich-text-editor
```

## Uso

### Importar o Componente

Após a instalação, você pode importar e usar o componente `RichTextEditor` em sua aplicação Vue.

```vue
<template>
  <div>
    <RichTextEditor
      v-model="editorContent"
      buttonClass="minha-classe-personalizada-botao"
      editorContainer="minha-classe-container-editor"
      editorClass="minha-classe-editor"
    />
  </div>
</template>

<script>
import RichTextEditor from 'rich-text-editor'; // ajuste o caminho se necessário

export default {
  components: {
    RichTextEditor,
  },
  data() {
    return {
      editorContent: '<p>Comece a editar...</p>',
    };
  },
};
</script>

<style>
.minha-classe-personalizada-botao {
  background-color: #f0f0f0;
  color: #333;
  border-radius: 4px;
}
.minha-classe-personalizada-botao.active {
  background-color: #007bff;
  color: white;
}
</style>
```

### Props

O componente `RichTextEditor` aceita as seguintes props:

- `content` (String): O conteúdo inicial do editor. O padrão é uma string vazia.
- `editorContainer` (String): Uma classe personalizada para o contêiner do editor.
- `editorClass` (String): Uma classe personalizada para a área do editor.
- `buttonClass` (String): Uma classe personalizada para os botões na barra de ferramentas.

### Métodos

- `toggleStyle(style)`: Alterna o estilo especificado (negrito, itálico, sublinhado) para o texto selecionado.
- `updateActiveStyles()`: Atualiza os estilos ativos com base na seleção atual.
- `updateContent()`: Emite o conteúdo atual do editor.

## Estilos

O componente vem com estilos básicos, mas você pode personalizar a aparência passando suas próprias classes por meio das props `editorClass` e `buttonClass`.

## Licença

Este projeto está licenciado sob a Licença MIT. Consulte o arquivo [LICENSE](LICENSE) para mais detalhes.

## Contribuindo

Se você gostaria de contribuir para este projeto, sinta-se à vontade para abrir uma issue ou enviar um pull request.