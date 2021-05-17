<template>
  <div>
    <div
      class="further-reading"
      v-if="isPreviousLinkDefined || isNextLinkDefined"
    >
      Further Reading:
    </div>
    <div class="d-flex justify-between flex-wrap">
      <div v-if="!isPreviousLinkDefined"></div>
      <btn
        v-if="isPreviousLinkDefined"
        classes="mt-10 mr-10 small"
        :link="previousArticleLink"
        >← {{ previousArticleTitle }}</btn
      >
      <btn
        v-if="isNextLinkDefined"
        classes="mt-10 small reverse"
        :link="nextArticleLink"
        >{{ nextArticleTitle }} →</btn
      >
      <div v-if="!isNextLinkDefined"></div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    title: {
      type: String,
      require: true,
    },
  },
  data() {
    return {
      posts: null,
      nextArticle: "",
      previousArticle: "",
      nextArticleLink: "",
      previousArticleLink: "",
      isPreviousLinkDefined: false,
      isNextLinkDefined: false,
    };
  },
  created() {
    this.posts = this.getAllArticlesSorted();
    const nxtIndex = this.getIndexOfCurrentArticle()[0].index + 1;
    const prvIndex = this.getIndexOfCurrentArticle()[0].index - 1;
    if (prvIndex >= 0) {
      this.previousArticleTitle = this.posts[prvIndex].frontmatter.title;
      this.previousArticleLink = this.posts[prvIndex].path;
      this.isPreviousLinkDefined = true;
    }
    if (nxtIndex < this.posts.length) {
      this.nextArticleTitle = this.posts[nxtIndex].frontmatter.title;
      this.nextArticleLink = this.posts[nxtIndex].path;
      this.isNextLinkDefined = true;
    }
  },
  methods: {
    // filter and return all posts with type "article", add property date and sort them by date published.
    getAllArticlesSorted(sorting = "DESC") {
      let posts = this.$site.pages
        .filter((page) => page.frontmatter.type === "article")
        .map((page, index) => {
          const path = page.path.split("/");
          page.date = path[1] + "-" + path[2] + "-" + path[3];
          page.index = index;
          return page;
        });
      // sort by the posts by providing 'sorting' options
      return this.sortPosts(posts, sorting);
    },
    sortPosts(posts, sorting) {
      return posts.sort((postA, postB) => {
        const dateA = postA.date.toUpperCase();
        const dateB = postB.date.toUpperCase();
        if (sorting === "DESC") {
          if (dateA > dateB) {
            return -1;
          }
          if (dateA < dateB) {
            return 1;
          }
          return 0;
        } else if (sorting === "ASC") {
          if (dateA < dateB) {
            return -1;
          }
          if (dateA > dateB) {
            return 1;
          }
          return 0;
        }
      });
    },
    getIndexOfCurrentArticle() {
      return this.posts.filter((post, index) => {
        if (post.frontmatter.title === this.title) {
          post.index = index;
          return post;
        }
      });
    },
  },
};
</script>

<style scoped>
.further-reading {
  font-size: 21px;
  font-weight: 600;
  margin-top: 25px;
}
</style>
