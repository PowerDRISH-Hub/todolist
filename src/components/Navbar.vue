<template>
  <header class="header">
    <div class="header__notes">
      <button @click="changeLang" class="header__lang" v-if="lang == 'ru'">UZ</button>
      <button @click="changeLang" class="header__lang" v-else>RU</button>
      <div class="container">
        <div class="header__change">
          <transition name="opp">
            <p class="header__p" v-if="header">{{words.appbartitle[lang]}}</p>
            <input
              type="text"
              :placeholder="words.appbarseacrch[lang]"
              class="header__input"
              v-model="search"
              v-else
            />
          </transition>
        </div>
      </div>
      <button class="header__search" @click="showSearch">
        <img src="@/assets/img/search.svg" alt="" v-if="header" />
        <img src="@/assets/img/close.svg" alt="" v-else />
      </button>
    </div>
  </header>
</template>

<script>
export default {
  name: "NavbarTodo",
  props: {
    lang: String,
  },
  inject:["words"],
  data() {
    return {
      search: "",
      header: true,
    };
  },
  watch: {
    search(val) {
      this.$emit("getSearch", val);
    },
  },
  methods: {
    showSearch() {
      this.header = !this.header;
    },
    changeLang(){
      this.$emit("changeLang", this.lang == "ru" ? "uz" : "ru")
    },
  },
};
</script>

<style>
</style>