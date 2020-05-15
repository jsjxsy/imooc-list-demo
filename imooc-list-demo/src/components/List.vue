<template>
  <div class="course">

    <Row :gutter="20">
      <i-col :xs="24" :sm="12" :md="8" :lg="8" v-for="(item) in lists" :key="item.id">
        <div class="course-item">
          <img :src="item.image" class="course-img">
          <div class="title_div">
            <span class="title">{{item.title}}</span>
          </div>
          <div class="subtitle">
            {{item.subtitle}}
          </div>
          <div :class="{time_title:true}">
            {{item.timespan}}
          </div>
        </div>
      </i-col>
    </Row>
    <Row>
      <i-col :xs="24" :sm="24" :md="24" :lg="24">
        <Page :total="totalCount" show-elevator @on-change="changePage" :page-size="6" :current="currentPage"/>
      </i-col>
    </Row>
  </div>
</template>

<script lang="js">
export default {
  name: 'List',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      currentPage: 1,
      totalCount: 1,
      lists: []
    }
  },
  mounted () {
    // if (!this.$route.query.curPage) { // url未指定页码数
    //   this.$router.push({
    //     path: 'list',
    //     query: {
    //       curPage: 1
    //     }
    //   })
    // } else { // url手动输入
    //   this.currentPage = this.$route.query.curPage
    this.getData()
    // }
    // window.vue = this
  },
  methods: {
    getData () {
      let param = {
        params: {
          curPage: this.currentPage
        }
      }
      const URL = 'http://imoocnote.calfnote.com/inter/getClasses.php?curPage=1'
      this.$Loading.start()
      this.axios.get(URL, param).then(response => {
        console.log('response: ' + JSON.stringify(response, null, '\t'))
        let data = response.data
        this.currentPage = parseInt(data.curPage)
        this.totalCount = data.totalCount
        this.lists = data.data
        this.$Loading.finish()
      }).catch(reason => { this.$Loading.error() })
    },
    changePage (currentPage) {
      console.log('currentPage: ' + currentPage)
      this.$router.push({
        path: 'list',
        query: {
          curPage: currentPage
        }
      })
    }
  },
  watch: {
    $route () {
      console.log('route jump')
      this.currentPage = parseInt(this.$route.query.curPage)
      this.getData()
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  h1, h2 {
    font-weight: normal;
  }

  ul {
    list-style-type: none;
    padding: 0;
  }

  li {
    display: inline-block;
    margin: 0 10px;
  }

  a {
    color: #42b983;
  }
</style>
