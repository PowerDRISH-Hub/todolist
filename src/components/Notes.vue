<template>
  <div class="changer">
    <div class="container changer__wrap">
      <h3 class="all__notes">
        {{ notes.length > 0 ? words.infobar[lang] : words.noinfobar[key] }}
      </h3>
      <button class="ch__btn" @click="change = !change">
        <div v-if="change" class="ch__cont">
          <img src="@/assets/img/flex.svg" alt="" />
          <span>{{ words.list[lang] }}</span>
        </div>
        <div v-else class="ch__cont">
          <img src="@/assets/img/grid.svg" alt="" />
          <span>{{ words.grid[lang] }}</span>
        </div>
      </button>
    </div>
    <div class="container">
      <div :class="[change ? grid : flex]" v-if="notes.length > 0">
        <Note
          v-for="(note, index) in notes"
          :key="index"
          :note="note"
          @changeNote="$emit('changeNote', note.id)"
          @delNote="$emit('delNote', note.id)"
          :lang="lang"
        />
      </div>
    </div>
  </div>
</template>

<script>
import Note from "@/components/Noteitem.vue";

export default {
  name: "NotesTodo",
  components: {
    Note,
  },
  props: {
    notes: {
      typeof: Array,
    },
    lang: String,
  },
  inject: ["words"],
  data() {
    return {
      change: true,
      flex: "changer__flex",
      grid: "changer__grid",
    };
  },
};
</script>

<style>
</style>