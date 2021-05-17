<template>
  <div>
    <router-link :to="link">
      <div class="post animated pulse-hover">
        <div
          class="post__image"
          :style="`background-image: url('${img}');`"
        ></div>
        <div class="post__text">
          <div class="post__text-title">
            <strong><slot name="title"></slot></strong>
          </div>
          <div class="post__text-date"><slot name="datePublished"></slot></div>
          <div class="post__tags">
            <tag-pills :small="true" :tags="tags" />
          </div>
          <!-- <div class="post__devider"></div> -->
          <div class="post__content">
            <div class="post__content-text">
              <slot name="description"></slot>
            </div>
            <div class="post__content-arrow">
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 25.21 24.37">
                <g id="Layer_3" data-name="Layer 3">
                  <g>
                    <polyline
                      points="13.24 2.59 22.98 12.5 13.8 21.86"
                      fill="none"
                      stroke="#40979d"
                      stroke-linecap="round"
                      stroke-linejoin="round"
                      stroke-width="4"
                    />
                    <polyline
                      points="2.33 2.32 12.06 12.24 2.89 21.6"
                      fill="none"
                      stroke="#40979d"
                      stroke-linecap="round"
                      stroke-linejoin="round"
                      stroke-width="4"
                    />
                  </g>
                </g>
              </svg>
            </div>
          </div>
        </div>
      </div>
    </router-link>
    <arrow-down v-if="enableArrowDown" />
  </div>
</template>

<script>
export default {
  props: {
    tags: {
      type: Array,
      required: true,
    },
    img: {
      type: String,
      required: true,
    },
    link: {
      type: String,
      required: true,
    },
    enableArrowDown: {
      type: Boolean,
      default: true,
    },
  },
};
</script>

<style scoped>
a:hover {
  text-decoration: none !important;
}
.post {
  display: flex;
  margin-top: 35px;
  margin-bottom: 35px;
  background-color: #d8d7c1;
  width: 100%;
  height: 300px;
  box-shadow: 0 4px 10px 0 rgb(0 0 0 / 20%), 0 4px 20px 0 rgb(0 0 0 / 19%);
  border-radius: 32px;
  overflow: hidden;
  cursor: pointer;
}

@media (max-width: 700px) {
  .post {
    height: 350px;
  }
}

@media (max-width: 700px) {
  .post {
    display: block;
  }
}

.post__image {
  flex: 1;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
}

@media (max-width: 700px) {
  .post__image {
    display: none;
  }
}

.post__text {
  flex: 1.3;
  align-self: center;
}

@media (max-width: 700px) {
  .post__text {
    margin-top: 21px;
  }
}

.post__text-title {
  font-size: 24px;
  margin-left: 50px;
  margin-right: 20px;
  line-height: 24px;
  color: #173353 !important;
}

@media (max-width: 700px) {
  .post__text-title {
    margin-left: 20px;
  }
}

.post__text-date {
  margin-left: 50px;
  margin-right: 20px;
  margin-top: 5px;
  font-size: 16px;
  font-style: italic;
  font-weight: 300;
  color: #173353 !important;
}

@media (max-width: 700px) {
  .post__text-date {
    margin-left: 20px;
  }
}

.post__devider {
  width: 130px;
  height: 8px;
  margin-top: 10px;
  margin-left: 50px;
  margin-right: 20px;
  background-color: #40979d;
  border-radius: 32px;
}

.post__tags {
  margin-top: 10px;
  margin-left: 50px;
  margin-right: 20px;
}

@media (max-width: 700px) {
  .post__devider {
    margin-left: 20px;
  }
  .post__tags {
    margin-left: 20px;
  }
}

.post__content {
  display: flex;
  justify-content: space-between;
  margin-top: 7px;
  margin-left: 50px;
  margin-right: 50px;
  font-size: 16px;
  font-weight: 400;
  line-height: 24px;
  color: #6d6e71 !important;
}

@media (max-width: 700px) {
  .post__content {
    margin-top: 6px;
    margin-right: 20px;
    margin-left: 20px;
  }
}

.post__content-text {
  margin-right: 20px;
  overflow: hidden;
  display: -webkit-box;
  -webkit-line-clamp: 6;
  -webkit-box-orient: vertical;
}

@media (max-width: 700px) {
  .post__content-text {
    margin-right: 10px;
    -webkit-line-clamp: 8;
  }
}

@media (max-width: 350px) {
  .post__content-text {
    -webkit-line-clamp: 7;
  }
}

.post__content-arrow {
  align-self: flex-end;
}

.post__content-arrow svg {
  width: 20px;
}
</style>
