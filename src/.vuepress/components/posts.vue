<template>
  <div>
    <single-post
      v-for="(post, index) in posts"
      :key="index"
      :tags="post.frontmatter.tags"
      :img="post.frontmatter.img"
      :link="post.path"
      :enableArrowDown="index < posts.length - 1 || enableLastArrowDown"
    >
      <template v-slot:title>
        {{ post.frontmatter.title }}
      </template>
      <template v-slot:datePublished>
        {{ post.frontmatter.published }}
      </template>
      <template v-slot:description>
        {{ post.frontmatter.description }}
      </template>
    </single-post>
    <div
      class="d-flex justify-between flex-wrap"
      v-if="getTheNumberOfPages() > 1"
    >
      <btn
        v-if="getTheCurrentPage() - 1 !== 0"
        :link="`?page=${Number(getTheCurrentPage()) - 1}`"
        classes="mt-10 mb-10 mr-10"
        :width="'width: 130px;'"
        >← Previous</btn
      >
      <div v-else></div>
      <btn
        v-if="getTheCurrentPage() < getTheNumberOfPages()"
        :link="`?page=${Number(getTheCurrentPage()) + 1}`"
        classes="mt-10 mb-10 mr-10"
        :width="'width: 130px;'"
        >Next →</btn
      >
      <div v-else></div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    pageSize: {
      type: Number,
      default: 10,
    },
    maxNumberOfPosts: {
      type: Number,
      default: -1,
    },
    enablePagination: {
      type: Boolean,
      default: true,
    },
    enableLastArrowDown: {
      type: Boolean,
      default: false,
    },
    filterBy: {
      type: String,
      default: "normal",
    },
  },
  data() {
    return {
      posts: [],
      assignNumberOfPosts: [],
    };
  },
  created() {
    this.initialisePosts();
  },
  methods: {
    initialisePosts() {
      // get the maximum number of posts if it's defined. If it's not defined, return all
      this.assignNumberOfPosts = this.sliceToMaximumNumberOfPosts(
        this.allPosts,
        this.maxNumberOfPosts !== -1
          ? this.maxNumberOfPosts
          : this.allPosts.length
      );
      // check if the query parametar ?page= in the URL is defined. If is undifined assign 1
      if (this.$route.query.page) {
        this.posts = this.paginatePosts(
          this.assignNumberOfPosts,
          this.pageSize,
          this.getTheNumberOfPages() > this.$route.query.page
            ? this.$route.query.page
            : this.getTheNumberOfPages()
        );
      } else {
        this.posts = this.paginatePosts(
          this.assignNumberOfPosts,
          this.pageSize,
          1
        );
      }
    },
    // filter and return all posts with type "article", add property date and sort them by date published.
    getAllArticlesSorted(sorting = "DESC") {
      let posts = this.$site.pages
        .filter((page) => page.frontmatter.type === "article")
        .map((page) => {
          const path = page.path.split("/");
          page.date = path[1] + "-" + path[2] + "-" + path[3];
          return page;
        });
      // if "tag" query parametar exist filter by tag
      if (this.filterBy !== "normal") {
        posts = posts.filter((post) => {
          for (let tag of post.frontmatter.tags) {
            if (
              tag.toLowerCase().replace(/\./g, "") ==
              this.filterBy.toLowerCase().replace(/\./g, "")
            ) {
              return post;
            }
          }
        });
      }
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
    sliceToMaximumNumberOfPosts(posts, maxNumberOfPosts) {
      return posts.slice(0, maxNumberOfPosts);
    },
    paginatePosts(posts, pageSize, pageNumber) {
      return posts.slice((pageNumber - 1) * pageSize, pageNumber * pageSize);
    },
    getTheNumberOfPages() {
      return Math.ceil(
        this.sliceToMaximumNumberOfPosts(
          this.allPosts,
          this.maxNumberOfPosts !== -1
            ? this.maxNumberOfPosts
            : this.allPosts.length
        ).length / this.pageSize
      );
    },
    getTheCurrentPage() {
      if (this.$route.query.page) {
        return this.$route.query.page;
      } else {
        return 1;
      }
    },
  },
  computed: {
    allPosts() {
      return this.getAllArticlesSorted();
    },
  },
  watch: {
    "$route.query.page": {
      handler(query) {
        if (query) {
          this.posts = this.paginatePosts(
            this.assignNumberOfPosts,
            this.pageSize,
            this.getTheNumberOfPages() > this.$route.query.page
              ? this.$route.query.page
              : this.getTheNumberOfPages()
          );
        }
      },
    },
  },
};
</script>

<style lang="scss" scoped></style>
