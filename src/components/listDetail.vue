<template>
    <div class="detail">
        <v-header goBack="true" :headTitle="listDetail.title"></v-header>
        <div class="detail-body" v-show="isShow">
            <audio v-if="listDetail.audio" autoplay="autoplay" loop="loop">
                你的浏览器不支持 <code>audio</code> 音频播放功能.
                <source :src="listDetail.audio" type="audio/mpeg" />
            </audio>
            <section v-html="listDetail.content"></section>
            <review :id="detailId"></review>
        </div>
        <loading :show="done"></loading>
    </div>
</template>

<script>
    import loading from '../components/loading.vue';
    import vHeader from '../components/header.vue';
    import review from '../components/review.vue';
    export default {
        data() {
          return {
            done: false,
            isShow: false,     // 只有当数据加载完成之后才能够实现出来
            detailId: '',
            identifier: '',    // 标识符 景点介绍模块为1 旅行百宝箱模块为0
            listDetail: {      // 详情列表信息
                title: '',
                content: '',
                audio: null
            }   
          }
        },
        components: {
            loading, vHeader, review
        },
        created() {
            this.detailId = this.$route.params.id;
            this.identifier =  this.$route.params.identifier || 0;
        },
        mounted() {
           this.initPage();
        },
        methods: {
            initPage() {
                let listDetailUrl = '';
                this.$data.done = true;
                if(this.identifier == 1) {        
                    listDetailUrl = `/zhan/querySSSOne?id=${this.detailId}`;   // 景点介绍调用的接口
                } else if(this.identifier == 2) {
                    listDetailUrl = `/zhan/queryServiceOne?id=${this.detailId}`;   // 外部交通，周边景点调用接口
                } else {
                    listDetailUrl = `/zhan/queryServiceList?type=${this.detailId}`;     // 景点地图调用的接口
                }

                this.$http.get(listDetailUrl).then((response) => {
                    this.done = false;
                    this.isShow = true;
                    let data = response.data.rows;
                    // 景点介绍
                    if(this.identifier == 1) {
                        this.listDetail.title = data[0].SS_TITLE;
                        this.listDetail.content = data[0].SS_CONTENT;
                        this.listDetail.audio = data[0].SS_VIDEO_URL;
                    } else {
                        // 旅行百宝箱
                        this.listDetail.title = data[0].INFO_TITLE;
                        this.listDetail.content = data[0].INFO_CONTENT;
                    }
                }, (response) => {
                    console.log('opps Is Error: ' + response);
                    this.done = false;
                })
            }
        }
    }
</script>

<style scoped lang="scss">
  .detail {
    text-align: left;
    .goback  {
      display: flex;
      align-items: center;
      width: 100%;
      height: 50px;
      color: rgb(35, 132, 232);
      background: #efefef;
      text-align: left;
      padding-left: 20px;
      margin-bottom:  10px;
    }
    .detail-body {
        width: 90%;
        padding: 10px;
        margin: 60px auto 40px;
    }
  }
</style>

