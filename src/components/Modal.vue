<template>
  <Transition name="show">
    <div class="modal" @click="closeModal">
      <div class="modal__content" @click.stop="">
        <form class="form">
          <h3 class="add__h">
            {{ !edit ? words.titlewindow[lang] : words.titlewindowedit[lang] }}
          </h3>
          <div class="form__input">
            <p class="input__title">Title</p>
            <input
              type="text"
              class="input"
              placeholder="Title"
              v-model="title"
            />
          </div>
          <div class="form__input">
            <p class="input__title">Content</p>
            <input
              type="text"
              class="input"
              placeholder="Content"
              v-model="content"
            />
          </div>
        </form>
        <div class="modal__btns">
          <button class="modal__btn cansel" @click="closeModal">{{words.closebtn[lang]}}</button>
          <button class="modal__btn add" v-if="!edit" @click="addNote">
            {{words.addbtn[lang]}}
          </button>
          <button class="modal__btn add" v-else @click="changeNote">
            {{words.editwindowbtn[lang]}}
          </button>
        </div>
      </div>
    </div>
  </Transition>
</template>

<script>
export default {
  name: "ModalTodo",
  props: {
    edit: Boolean,
    currentId: Number,
    editNote: Object,
    lang: String,
  },
  inject:["words"],
  data() {
    return {
      title: "",
      content: "",
      id: 1,
    };
  },
  methods: {
    closeModal() {
      this.$emit("closeModal", false);
      this.title = "";
      this.content = "";
    },
    addNote() {
      if (this.title != "" && this.content != "") {
        const item = {
          id: this.id++,
          title: this.title,
          desc: this.content,
          date: new Date().toLocaleDateString(),
        };
        this.$emit("addNote", item);
        this.title = "";
        this.content = "";
      }
    },
    changeNote() {
      if (this.title != "" && this.content != "") {
        const editedNote = {
          id: this.editNote.id,
          title: this.title,
          desc: this.content,
          date: new Date().toLocaleDateString(),
        };
        this.$emit("editedNote", editedNote);
        this.closeModal();
      }
    },
  },
};
</script>

<style>
</style>