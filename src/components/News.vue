<template>
  <div>
    <div class="hello notification is-primary">
      <h1 class="title is-4">V-News</h1>
      <p class="subtitle">Get the lastst news from ur fav sources</p>
      <label class="label"> choose ur source please :) </label>
      <br />
      <div class="select is-warning">
        <select v-if="sources.length > 0" v-model="src" @change="getIndex();getSrcId()">
          <option v-for="(src) in sources" :key="src.id">
            {{src.name}}
          </option> 
        </select>
      </div>
      <div class="info" v-if="index">
        <div class="tags has-addons">
          <span class="tag" v-bind="src">WebSite </span>
          <a class="tag is-danger" :href="sources[index].url">Go</a>
        </div>
        <div class="tags has-addons">
          <span class="tag" v-bind="src">language</span>
          <span class="tag is-danger" >{{sources[index].language}}</span>
        </div>
        <article class="message is-danger">
          <div class="message-header">
            About The Source
          </div>
          <div class="message-body">
            {{sources[index].description}}
          </div>
        </article>
      </div>
    </div>
    <div class="card" v-if="Array.isArray(last)" v-for="lastst in last" :key="lastst.title">
      <div class="card-image">
        <figure class="image is-4by3">
          <img :src="lastst.urlToImage" alt="Placeholder image">
        </figure>
      </div>
      <div class="card-content">
        <div class="media">
          
          <div class="media-content">
            <p class="title is-4">{{lastst.title}}</p>
            <p class="subtitle is-6">{{lastst.author}}</p>
          </div>
        </div>

        <div class="content">
          {{lastst.description}} 
          <a class="readmore" :href="lastst.url">Read More ...</a>
          <br>
          <time datetime="2016-1-1">{{lastst.publishedAt}}</time>
        </div>
      </div>
    </div>
    <footer class="notification is-primary">
      <p>Made By Geek &amp; shakir</p>
    </footer>
  </div>
</template>

<script>
export default {
  name: 'News',
  data () {
    return {
      apiKey : '9a9dbe2a5aad47beb2fcb63bb4a4b169',
      sources : '',
      src : '',
      index : null,
      srcId : '',
      last:'',
    }
  },

  methods : {
    getData () {
      this.$http.get(`https://newsapi.org/v2/sources?apiKey=${this.apiKey}`)
        .then (res => this.sources = res.data.sources)
    },
    getIndex () {
      for (let src in this.sources){
        if (this.sources[src].name == this.src){
          this.index = src;
          break;
        }
      }
      this.getSrcId();
      this.getLast();
    },
    getLast () {
      this.$http.get(`https://newsapi.org/v2/top-headlines?sources=${this.srcId}&apiKey=${this.apiKey}`)
        .then (res => this.last = res.data.articles)
    },
    getSrcId () {
      this.srcId = this.sources[this.index].id
    }
  },
  mounted () {
    this.getData()
  }
}
</script>

<style scoped>
  #id {display: none}
  .hello {
    margin: 0px auto;
    text-align: center;
  }
  .notification {
    box-shadow: 0px 7px 20px #dbdbdb;
  }
  .card {
    width: 30%;
    margin: 10px 1.5%;
    float: left;
    height: 726px;
  }
  p{
    margin-bottom: 22px
  }
  label {margin-bottom: 15px}
  .tags {
    display: inline-table;
    margin-top: 15px;
  }
  .tags:not(:first-of-type){
    margin-left: 46px
  }
  .message {
    width: 500px;
    text-align: center;
    margin: auto;
    box-shadow: 3px 3px 20px -5px black
  }

  .readmore {
    margin: 10px 0;
    display:block;
  }
  footer {
    position: fixed;
    bottom: 0px;
    width: 100vw;
    text-align: center;
    padding: 10px 0 0 0;
  }
  @media only screen and (max-width: 767px) and (min-width: 575.98px)  {
    .card {
      width: 40%;
      margin:5%
    }
    .message {
      width: 253px;
    }
  }

  @media only screen and (max-width: 575.98px)  {
    .card {
      width: 80%;
      margin:10%
    }
    .message {
      width: 253px;
    }
  }
</style>
