<template>
  <div class="details">
    <div class="time_d">
      <matching-xq :m-list="classify"></matching-xq>
      <div class="time"><span>回复时间：{{answerTime}}</span><i></i></div>
    </div>
    <div class="margin_bottom"></div>
    <div class="details_xq">
      <div class="name_input">
        <span class="name">回复详情</span>
      </div>
      <div class="name_input">
        <span class="name">姓名</span>
        <div class="_input">{{classify.user_name}}</div>
      </div>
      <div class="name_input">
        <span class="name">联系方式</span>
        <div class="_input">{{classify.phone}}</div>
      </div>
    </div>
    <div class="margin_bottom"></div>
    <div class="details_xq">
      <div class="name_textarea">
        <span class="name">详情描述</span>
        <div class="_textarea">{{classify.answer_content}}</div>
      </div>
    </div>
    <div class="margin_bottom"></div>
    <div class="details_xq">
      <div class="name_fileImage">
        <span class="name">附件</span>
        <div class="fileImage_w floatClear">
          <div class="_fileImage_div left J_previewer-img" v-for="(picL, index) in list" :style="{'background-image': 'url('+picL.src+')'}" @click="show(index)"></div>
        </div>
        <div v-transfer-dom>
          <previewer :list="list" ref="previewer" :options="options"></previewer>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import $$ from 'assets/js/common';
  import MatchingXq from 'components/matchingXq/matchingXq2';
  import { Previewer, TransferDom } from 'vux'

  export default {
    name: 'matchingList',
    components: {
      MatchingXq,
      Previewer
    },
    directives: {
      TransferDom
    },
    data() {
      return {
        classify: {
          answer_time: new Date()
        },
        options: {
          getThumbBoundsFn (index) {
            let thumbnail = document.querySelectorAll('.J_previewer-img')[index]
            let pageYScroll = window.pageYOffset || document.documentElement.scrollTop
            let rect = thumbnail.getBoundingClientRect()
            return {x: rect.left, y: rect.top + pageYScroll, w: rect.width}
          },
          isClickableElement : function(el){
            return true;
          }
        },
        list: []
      }
    },
    computed: { //计算
      answerTime() {
        return dateFormat(this.classify.answer_time, 'YYYY-MM-DD HH:mm:ss')
      },
    },
    props: { //继承

    },
    methods: {
      show (index) {
        this.$refs.previewer.show(index)
      }
    },
    mounted: function() { //类似于回调函数(初次实例化完成后调用)
      //this.pulldownFn();
      var v_this = this;
      $$.post('/api/wxanswer/details', {
        "token": localStorage.getItem('userToken'),
        "data": {
          "answerId": v_this.$route.query.id
        }
      }, function(data) {
        if(data.status = '0') {
          let res = data.data
          v_this.classify = res;
          for(let n in res) {
            if(n.indexOf('pic') != -1) {
              v_this.list.push({
                src:'http://image.dhbigdata.cn/dfun/' + res[n]
              });
            }
          }
        }
      });
    },
    //数据更改后调用
    watch: {

    }
  }
</script>

<style lang="less">
  @import url("./details");
</style>
