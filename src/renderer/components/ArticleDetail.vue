<template>
  <div class="article-detail">
    <div class="content-wrapper">
      <div class="article-toolbar">
        <div class="site-info" v-if="article">
          <div class="wrap">
            <button class="btn btn-toolbar">
              <span v-if="article.favicon" class="favicon-wrap">
                <img :src="article.favicon" width="16" height="16">
              </span>
              <span :class="{ 'ml-4': article.favicon }">{{ article.sitetitle }}</span>
            </button>
          </div>
        </div>
        <div class="article-buttons" v-if="article">
          <div class="wrap">
            <button class="btn btn-toolbar" @click="markFavourite">
              <feather-icon name="star" :filled="article.favourite"></feather-icon>
            </button>
          </div>
          <div class="wrap">
            <button class="btn btn-toolbar" @click="markRead">
              <feather-icon name="circle" :filled="article.read"></feather-icon>
            </button>
          </div>
          <div class="wrap">
            <a :href="article.url" class="btn btn-toolbar js-external-link">
              <feather-icon name="external-link"></feather-icon>
            </a>
          </div>
        </div>
      </div>
      <div class="article-contentarea  px-4" v-if="article">
        <h2>
          <strong>{{ article.title }}</strong><br/>
          <small><span v-if="article.date_published">{{ article.date_published }} </span> <span v-if="article.author">by {{ article.author }}</span>  <strong v-if="article.date_published || article.date_published">&#183;</strong> {{ article.readtime }}</small>
        </h2>
        <div class="article-detail" v-html="article.content"></div>
      </div>
      <div class="article-contentarea loading-state px-4" v-if="!article">
        <div class="bouncing-loader" v-if="loading">
          <div></div>
          <div></div>
          <div></div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    id: {
      type: String
    },
    article: {
      type: Object
    },
    loading: {
      type: Boolean
    }
  },
  methods: {
    markFavourite () {
      if (this.article.favourite) {
        this.$store.dispatch('markUnfavourite', this.$route.params.id)
      } else {
        this.$store.dispatch('markFavourite', this.$route.params.id)
      }
      this.article.favourite = !this.article.favourite
    },
    markRead () {
      if (this.article.read) {
        this.$store.dispatch('markUnread', this.$route.params.id)
      } else {
        this.$store.dispatch('markRead', this.$route.params.id)
      }
      this.article.read = !this.article.read
    }
  }
}
</script>
<style lang="scss">
.article-detail {
  position: relative;
  flex-grow: 1;
  height: 100%;
}

.content-wrapper {
  overflow: hidden;
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
}

.article-toolbar {
  display: flex;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 3;
  width: 100%;
  border-bottom: 1px solid #e3e3e3;
  height: 41px;
}

.site-info,
.article-buttons {
  display: block;
  position: absolute;
  top: 0;
  width: 270px;
  height: 40px;
  z-index: 1;
  background-image: linear-gradient(to right, rgba(255,255,255,0) 0%, #fff 10%);
}

.site-info {
  left: 0;

  .wrap {
    float: left;
  }
}

.article-buttons {
  right:0 ;

  .wrap {
    float: right;
  }
}

.article-contentarea {
  background-color: #fff;
  display: block;
  position: absolute;
  height: auto;
  bottom: 0;
  left: 0;
  right: 0;
  top: 41px;
  margin: 0;
  overflow-x: hidden;
  overflow-y: auto;
  z-index: 2;
  padding: 15px;

  h2 {
    small {
      font-size: 14px;
    }
  }
}

.article-detail {
  img {
    display: block;
    max-width: 100%;
    margin-bottom: 15px;
  }

  h2 {
    margin-bottom: 25px;
  }

  iframe {
    display:block;
    width: 100%;
    height: 500px;
    border: 0;
  }

  .col {
    padding-left: 0;
    padding-right: 0;
  }
}

.btn-toolbar {
  color: black;
  display: block !important;
  z-index: 2;
  background: transparent;
  border: none;
  border-radius: 0;
  width: 44px;
  height: 40px;
  padding: 0;
  position: relative;

  &:hover {
    color: black;
  }
}

.favicon-wrap {
  position: absolute;
  box-shadow: none;
  height: 20px;
  width: 20px;
  left: 12px;
  top: 17px;
  display: flex;
  align-items: center;
  pointer-events: none;
  z-index: 0;
}

.loading-state {
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
