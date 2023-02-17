<template>
  <Navbar @getSearch="search = $event" :lang="lang" @changeLang="changeLang" />
  <Notes :notes="filterNotes" @changeNote="changeNote" @delNote="delNote" :lang="lang" />
  <AddBtn @click="openModal" :lang="lang" />
  <Modal
    @editedNote="editedNote"
    @closeModal="closeModal"
    @addNote="addNote"
    v-show="modalWind"
    :edit="edit"
    :currentId="currentId"
    :editNote="editNote"
    :lang="lang"
  />
</template>

<script>
import Navbar from "@/components/Navbar.vue";
import Notes from "@/components/Notes.vue";
import AddBtn from "@/components/AddBtn.vue";
import Modal from "@/components/Modal.vue";
import langs from "@/lang.js";
export default {
  name: "App",
  components: {
    Navbar,
    Notes,
    AddBtn,
    Modal,
  },
  data() {
    return {
      search: "",
      currentId: 1,
      notes: [
        { id: 1, title: "title", desc: "lorem ipsum", date: new Date() },
        { id: 2, title: "title", desc: "lorem ipsum", date: new Date() },
        { id: 3, title: "title", desc: "lorem ipsum", date: new Date() },
      ],

      modalWind: false,
      editNote: {},
      edit: false,
      lang: "ru",
      langwords: {},
    };
  },
  mounted() {
    this.getNotes();
    localStorage.lang = localStorage.lang || "ru";
    this.lang = localStorage.lang || "ru";
    this.langwords = langs;
    localStorage.words = JSON.stringify(this.langwords);
  },
  provide() {
    return {
      words: localStorage.words ? JSON.parse(localStorage.words) : langs,
    };
  },

  computed: {
    filterNotes() {
      return this.search
        ? this.notes.filter((item) =>
            item.title.toLowerCase().includes(this.search.toLowerCase())
          )
        : this.notes;
    },
  },
  methods: {
    addNote(item) {
      this.notes.push(item);
      this.modalWind = false;
    },
    openModal() {
      this.edit = false;
      this.modalWind = true;
    },
    closeModal() {
      this.modalWind = false;
    },
    changeNote(id) {
      this.edit = this.modalWind = true;
      let pickedNote = this.notes.find((note) => note.id == id);
      this.editNote = pickedNote;
    },
    editedNote(noteEdited) {
      this.notes.forEach((note) => {
        if (note.id == noteEdited.id) {
          note.title = noteEdited.title;
          note.desc = noteEdited.desc;
          note.date = noteEdited.date;
        }
      });
    },
    delNote(id) {
      let index = this.notes.findIndex((note) => note.id == id);
      this.notes.splice(index, 1);
    },
    getNotes() {
      const localNotes = localStorage.notes;
      if (localNotes) {
        this.notes = JSON.parse(localNotes);
      }
    },
    changeLang(val) {
      this.lang = localStorage.lang = val;
      console.log(this.lang);
    },
  },
  watch: {
    notes: {
      handler() {
        localStorage.notes = JSON.stringify(this.notes);
      },
      deep: true,
    },
  },
};
</script>

<style>
</style>
