<template>
  <div class="tags" :class="`${small ? 'tags-small' : 'tags-large'}`">
    <router-link
      v-for="(tag, index) in tags"
      :key="index"
      :to="
        `${
          tag == 'Vue.js'
            ? tagDetails[0].link
            : tag == 'JavaScript'
            ? tagDetails[1].link
            : tag == 'FrontEnd'
            ? tagDetails[2].link
            : tagDetails[3].link
        }`
      "
    >
      <div
        class="d-inline-block animated pulse-hover"
        :style="
          `background-color:${
            tag == 'Vue.js'
              ? tagDetails[0].bg
              : tag == 'JavaScript'
              ? tagDetails[1].bg
              : tag == 'FrontEnd'
              ? tagDetails[2].bg
              : tagDetails[3].bg
          }; color:${
            tag == 'Vue.js'
              ? tagDetails[0].txt
              : tag == 'JavaScript'
              ? tagDetails[1].txt
              : tag == 'FrontEnd'
              ? tagDetails[2].txt
              : tagDetails[3].txt
          }`
        "
      >
        {{ tag }}
      </div>
    </router-link>
  </div>
</template>

<script>
export default {
  props: {
    tags: {
      type: Array,
      required: true,
    },
    small: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      tagDetails: [
        { bg: "#40979d", txt: "#ffffff", link: "/vuejs" },
        { bg: "#FFD700", txt: "#173353", link: "/javascript" },
        { bg: "#173353", txt: "#ffffff", link: "/frontend" },
        { bg: "#ee5f53", txt: "#ffffff", link: "/other" },
      ],
    };
  },
};
</script>

<style scoped>
.tags div:hover {
  opacity: 0.8;
}

.tags-large > a:not(:last-child) {
  margin-right: 10px;
}

.tags-large div {
  margin-bottom: 10px;
  padding: 8px 17px;
  border-radius: 25px;
  font-size: 14px;
  color: #ffffff;
}

.tags-small > a:not(:last-child) {
  margin-right: 6px;
}

.tags-small div {
  margin-bottom: 6px;
  padding: 5px 10px;
  border-radius: 18px;
  font-size: 12px;
  color: #ffffff;
}

.d-inline-block {
  display: inline-block;
}
</style>
