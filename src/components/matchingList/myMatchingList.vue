<template>
  <div class="matching_list my_matching_list">
    <scroller lock-x height="100%" ref="myscroller" use-pullup @on-pullup-loading="pullupFn" :pullup-config="pullupConfig" v-model="scrollerStatus" v-show="classifyList">
      <div>
        <div class="margin_bottom"></div>
        <div v-for="list in classifyList">
          <matching-xq :m-list="list">
            <div slot="meta">
               <div class="div_2">
                <span class="chakan"><i></i>{{list.browse_count}}</span>
                <span class="xihuan"><i></i>{{list.fav_count}}</span>
                <span class="shijian">{{ list.create_time | formatDate }}</span>
              </div>
            </div>
          </matching-xq>
          <div class="wdhf" @click="pushFn(list.answerId)">
            <div>
              <div class="name">我的回复</div>
              <div class="time"><span>回复时间：{{ list.answer_time | formatDate }}</span><i></i></div>
            </div>
          </div>
        </div>
        <div class="jzwP" v-if="classifyList.length==dataCount">加载完了，共{{dataCount}}条</div>
      </div>
    </scroller>
    <no-data class="matching_no_data" v-if="!classifyList" :noText="noDataText"></no-data>
  </div>
</template>
<script>
  import { Scroller } from 'vux';
  import MatchingXq from '../matchingXq/matchingXq2';
  import NoData from '../noData/noData';
  import { pulldownConfig, pullupConfig, scrollerStatus, pulldownF, pullupF, watchF } from './index';

  export default {
    name: 'matchingList',
    components: {
      Scroller,
      NoData,
      MatchingXq
    },
    data() {
      return {
        myUrl: '/api/wxdemand/getClassifyList',
        pulldownConfig,
        pullupConfig,
        scrollerStatus,
        classifyList: '',
        noDataText: '暂无数据',
        pageNum: 1,
        pageSize: 5,
        dataCount: 0,
      }
    },
    computed: { //计算
      noDataShow() {
        return this.classifyList.length ? false : true;
      }
    },
    props: { //继承
      demandType: { //需求类型
        default: '', //---空：全部，0：需求，1：接单"
      },
      auditStatus: {
        default: '', //---空：全部，0：进行中，3：结束"
      },
      readSort: {
        default: '', //---10：阅读排序降序，11：阅读排序升序，20：收藏数排序降序：21：收藏数排序升序，30：时间排序降序，31：时间排序升序;
      }
    },
    methods: {
      pushFn(id) {
        console.log(id)
        this.$router.push('/usercenter/reply/details?id=' + id);
      },
      //下拉刷新
      pulldownFn() {
        var v_this = this;
        pulldownF(this, v_this.myUrl, {
          version: "1.0",
          pageSize: v_this.pageSize,
          pageNum: 1,
          token: localStorage.getItem('userToken'),
          data: {
            "demandType": v_this.demandType,
            "auditStatus": v_this.auditStatus,
            "sort": v_this.readSort
          }
        });
      },
      //上拉加载更多
      pullupFn() {
        var v_this = this;
        v_this.pageNum++;
        pullupF(this, v_this.myUrl, {
          version: "1.0",
          pageSize: v_this.pageSize,
          pageNum: v_this.pageNum,
          token: localStorage.getItem('userToken'),
          data: {
            "demandType": v_this.demandType,
            "auditStatus": v_this.auditStatus,
            "sort": v_this.readSort
          }
        });
      }
    },
    mounted: function() { //类似于回调函数(初次实例化完成后调用)
      this.$nextTick(() => {
        this.$refs.myscroller.reset({
          top: 0
        })
      });
      if(this.$route.path == '/usercenter/release') {
        this.myUrl = '/api/personalCenter/demandUserList';
      }
      if(this.$route.path == '/usercenter/favorite') {
        this.myUrl = '/api/personalCenter/demandFavList';
      }
      if(this.$route.path == '/usercenter/reply') {
        this.myUrl = '/api/personalCenter/demandAnswerList';
      }
      console.log('请求地址:', this.myUrl)
      //初始数据请求
      this.pulldownFn();
    },
    //数据更改后调用
    watch: {
      demandType: function(newV, oldV) {
        watchF(this, newV, oldV);
      },
      auditStatus: function(newV, oldV) {
        watchF(this, newV, oldV);
      },
      readSort: function(newV, oldV) {
        watchF(this, newV, oldV);
      },
    },
    activated() {
      this.$refs.scroller.reset()
    }
  }
</script>

<style lang="less">
  @import url("../../assets/styles/less");
  .matching_list {
    height: ~'calc(100vh - 0.60rem)';
    overflow: hidden;
    padding-bottom: 1.0rem;
    position: relative;
    .my-container {
      line-height: 40px;
    }
    .margin_bottom {
      &:after {
        content: '';
        display: block;
        height: 0.20rem;
        background-color: #ebebeb;
      }
    }
  }

  .matching_no_data {
    margin-top: 0.20rem;
    width: 100%;
    height: 100%;
  }

  .my_matching_list {
    .matching_list_xq {
      padding-bottom: 0.20rem;
      &:after {
        display: none;
      }
    }
    .wdhf {
      width: 100%;
      padding-left: 0.30rem;
      background-color: @colorf;
      &:after {
        content: '';
        display: block;
        margin-left: -0.28rem;
        margin-right: -0.28rem;
        height: 0.20rem;
        background-color: #ebebeb;
      }
      >div {
        border-top: 1px solid @SecondaryText;
        vertical-align: middle;
        overflow: hidden;
        .name {
          float: left;
          .font32;
          color: @color6;
          line-height: 0.90rem;
        }
        .time {
          float: right;
          font-size: 0.24rem;
          color: @color9;
          line-height: 0.90rem;
          vertical-align: middle;
          i {
            display: inline-block;
            width: 0.16rem;
            height: 0.28rem;
            background: url(r.png) no-repeat;
            .bg100;
            margin-right: 0.28rem;
            margin-left: 0.22rem;
            margin-bottom: 0.06rem;
            vertical-align: middle;
          }
        }
      }
    }
  }

  .jzwP {
    text-align: center;
    height: 40px;
    line-height: 40px;
  }
</style>
