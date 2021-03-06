<template>
  <div class="articles-list">
    <div class="articles-inner">
      <form class="search-form">
        <div class="search-input input-group mb-0">
          <div class="input-group-prepend">
            <span class="input-group-text">
              <feather-icon name="search"></feather-icon>
            </span>
          </div>
          <input type="text" class="form-control" placeholder="Search" aria-label="Search" v-model="search">
        </div>
      </form>
      <div class="articles">
        <div class="list-group">
          <router-link v-if="articles.length > 0" :to="`/article/${article._id}`" :class="{ 'article-read': article.read }" class="list-group-item list-group-item-action flex-column align-items-start" v-for="article in filteredArticles" :key="article._id">
            <div class="d-flex w-100 justify-content-between mb-3">
              <small><img v-if="article.meta.favicon" :src="article.meta.favicon" width="16" height="16"> {{ article.meta.title }}</small>
              <small>{{ article.pubdate }}</small>
            </div>
            <h6><strong>{{ article.title }}</strong></h6>
          </router-link>
          <div class="no-articles" v-if="articles.length === 0">
            No articles available
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import _ from 'lodash'

const filters = {
  search: (articles, search) => articles.filter(article => article.title.match(search)),
  unread: articles => articles.filter(article => !article.read),
  read: articles => articles.filter(article => article.read),
  favourites: articles => articles.filter(article => article.favourite),
  feed: (articles, feed) => articles.filter(article => article.feed_id === feed),
  all: articles => articles
}

export default {
  data () {
    return {
      search: null
    }
  },
  props: {
    type: {
      type: String,
      default: 'all'
    },
    feed: {
      type: String
    }
  },
  watch: {
    search (val) {
      if (val) {
        this.$emit('type-change', 'search')
      }
    }
  },
  computed: {
    articles () {
      const orderedArticles = _.orderBy(this.$store.state.Article.articles, ['pubDate'], ['desc'])
      return orderedArticles
    },
    filteredArticles () {
      if (this.type !== 'feed' && this.type !== 'search') {
        return filters[this.type](this.articles)
      }
      if (this.type === 'search') {
        return this.articles.filter(article => article.title.toLowerCase().match(this.search.toLowerCase()))
      }
      return filters[this.type](this.articles, this.feed)
    }
  }
}
</script>
<style lang="scss">
.articles-inner {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  overflow:hidden;
}

.articles-list {
  position: relative;
  flex-grow: 0;
  width: 350px;
  border-right: 1px solid rgba(0, 0, 0, 0.1);
  height: 100%;
}

.articles-list:after {
  content: '';
  position: absolute;
  bottom: 0;
  width: 100%;
  height: 25px;
  background: linear-gradient(rgba(255, 255, 255, 0.001), white ); /* transparent keyword is broken in Safari */
  pointer-events: none;
}

.articles {
  position: absolute;
  top: 41px;
  bottom: 0;
  left: 0;
  right: 0;
  overflow-y: auto;
  background: #fff;
  overflow-x: hidden;
}

.no-articles {
  display: flex;
  height: 90vh;
  justify-content: center;
  align-items: center;
}

.article-read {
  opacity: 0.7;
}

.search-input {
  position: absolute;
  top: 0;
  bottom: 0;

  .input-group-text {
    background: none;
    border: 0;
  }
}

.search-form {
  position: absolute;
  left: 0;
  right: 0;
  top: 0;
  margin: 0;
  border-bottom: 1px solid transparent;
  border-color: #dcdee0;
  background-color: #FFFFFF;
  height: 41px;

  .form-control {
    border-radius: 0;
    border: 0;

    &:focus {
      box-shadow: none;
      outline: 0;
    }
  }
}
</style>
