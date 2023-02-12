<template>
  <div>
    <div class="toolbar">
      <div class="toolbar__list">
        <button @click="formatDoc('undo')" class="toolbar__item">
          <icon-back />
        </button>
        <button @click="formatDoc('redo')" class="toolbar__item">
          <icon-next />
        </button>
        <button @click="formatDoc('fontSize', '6')" class="toolbar__item">
          <icon-title />
        </button>
        <button @click="formatDoc('fontSize', '3')" class="toolbar__item">
          <icon-paragraph />
        </button>
        <button @click="addImage" class="toolbar__item">
          <icon-image />
        </button>
        <button @click="copyHtml" class="toolbar__item toolbar__item--text">
          Скопировать HTML
        </button>
      </div>
    </div>
    <div
        class="editor"
        v-html="innerValue"
        @input="updateInput"
        contenteditable
        spellcheck="true"
    ></div>
  </div>
</template>

<script>
import IconBack from "@/components/icons/IconBack.vue";
import IconNext from "@/components/icons/IconNext.vue";
import IconTitle from "@/components/icons/IconTitle.vue";
import IconParagraph from "@/components/icons/IconParagraph.vue";
import IconImage from "@/components/icons/IconImage.vue";

export default {
  name: "WysiwygEditor",
  components: {IconImage, IconParagraph, IconTitle, IconNext, IconBack},
  props: ['modelValue'],
  data() {
    return {
      innerValue: this.modelValue || '<p><br></p>',
    }
  },
  mounted() {
    document.execCommand('defaultParagraphSeparator', false, 'p')
  },
  methods: {
    updateInput(event) {
      this.$emit("update:modelValue", event.target.innerHTML);
    },
    formatDoc(cmd, value=null) {
      value ? document.execCommand(cmd, false, value) : document.execCommand(cmd)
    },
    addImage() {
      const url = prompt('Вставить ссылку')
      this.formatDoc('insertImage', url)
    },
    copyHtml() {
      navigator.clipboard.writeText(this.modelValue)
          .then(() => {
            alert('Скопировано в буфер обмена')
          })
          .catch(err => {
            console.log('Что-то пошло не так', err);
          });
    }
  }
}
</script>
