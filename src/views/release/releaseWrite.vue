<template>
  <div class="issue">
    <div class="name_input_w">
      <div class="name_input name_a" @click="selectType">
        <span class="name">供需类型</span>
        <div class="_input"><i></i><span>{{ctypeT[submitData.demand_type]}}</span></div>
      </div>
    </div>
    <div class="name_input_w">
      <div class="name_input name_a" @click="selectCate">
        <span class="name">分类</span>
        <div class="_input"><i></i><span>{{classifyT[submitData.demand_category]}}</span></div>
      </div>
    </div>

    <div class="name_input_w2">
      <div class="name_input">
        <span class="name">标题</span>
        <div class="_input">
          <input class="textOverflow" type="text" v-model="submitData.demand_title" :placeholder="placeholder.required + placeholder.title" @change="nameFn2" @input="nameFnInput2" />
        </div>
      </div>
      <div class="name_input">
        <span class="name">姓名</span>
        <div class="_input">
          <input class="textOverflow" type="text" name="name" id="name" :placeholder="placeholder.required + placeholder.username" v-model="submitData.user_name" @change="nameFn" />
        </div>
      </div>
      <div class="name_input">
        <span class="name">联系方式</span>
        <div class="_input">
          <input class="textOverflow" type="text" name="" id="" value="" :placeholder="placeholder.required + placeholder.phone" v-model="submitData.phone" @change="checkPhone" />
        </div>
      </div>
      <div class="name_input">
        <span class="name">报酬（原价）</span>
        <div class="_input">
          <input class="textOverflow" type="number" :placeholder="placeholder.required + placeholder.cost" v-model="submitData.original_cost" style="padding-right: 0;" @change="numberFn('original_cost')" />
        </div>
      </div>
      <div class="name_input">
        <span class="name">报酬（现价）</span>
        <div class="_input">
          <input class="textOverflow" type="number" :placeholder="placeholder.required + placeholder.price" v-model="submitData.current_price" @change="numberFn('current_price')" />
        </div>
      </div>
    </div>
    <div class="name_input_w2">
      <div class="name_input">
        <span class="name">截止日期</span>
        <div class="_input _date">
          <i></i>
          <datetime v-model="submitData.end_time_desc" format="YYYY-MM-DD HH:mm" @on-change="dataFn"></datetime>
        </div>
      </div>
    </div>
    <div class="name_textarea_w">
      <div class="name_textarea"> <span class="name">详情描述</span>
        <div class="_textarea"> <textarea :placeholder="placeholder.required + placeholder.description" v-model="submitData.description" @input="detailsFn"></textarea> </div>
      </div>
    </div>
    <input class="_button" type="button" id="" value="立即发布" @click="submitFn" />
    <!--右滑弹窗-->
    <popup class="popup_w" v-model="gx_show" position="right" width="100%">
      <div class="margin_bottom"></div>
      <div class="name_input_w">
        <div class="name_input name_a" :class="{'active':submitData.demand_type==0}" @click="submitData.demand_type=0">
          <span class="name">需求</span>
          <div class="_input"> <i></i> </div>
        </div>
        <div class="name_input name_a" :class="{'active':submitData.demand_type==1}" @click="submitData.demand_type=1">
          <span class="name">接单</span>
          <div class="_input"> <i></i> </div>
        </div>
      </div>
      <input type="button" value="完成" @click="goBack" />
    </popup>
    <popup class="popup_w" v-model="gx_show2" position="right" width="100%">
      <div class="margin_bottom"></div>
      <div class="name_input_w">
        <div class="name_input name_a" :class="{'active':submitData.demand_category==0}" @click="submitData.demand_category=0">
          <span class="name">精准营销</span>
          <div class="_input"> <i></i> </div>
        </div>
        <div class="name_input name_a" :class="{'active':submitData.demand_category==1}" @click="submitData.demand_category=1">
          <span class="name">数据报告</span>
          <div class="_input"> <i></i> </div>
        </div>
        <div class="name_input name_a" :class="{'active':submitData.demand_category==2}" @click="submitData.demand_category=2">
          <span class="name">数据交易</span>
          <div class="_input"> <i></i> </div>
        </div>
        <div class="name_input name_a" :class="{'active':submitData.demand_category==3}" @click="submitData.demand_category=3">
          <span class="name">API</span>
          <div class="_input"> <i></i> </div>
        </div>
        <div class="name_input name_a" :class="{'active':submitData.demand_category==4}" @click="submitData.demand_category=4">
          <span class="name">其他定制</span>
          <div class="_input"> <i></i> </div>
        </div>
      </div>
      <input type="button" value="完成" @click="goBack" />
    </popup>

  </div>
</template>
<script>
  import $$ from 'assets/js/common';
  import { Datetime, Popup } from 'vux';
  import { mapGetters } from 'vuex';
  export default {
    name: 'reply',
    components: {
      Datetime,
      Popup,
    },
    data() {
      return {
        gx_show: false,
        gx_show2: false,
        ctypeT: ['需求', '接单'],
        classifyT: ['精准营销', '数据报告', '数据交易', 'API', '其他定制'],
        //minHour:dateFormat(new Date(), 'HH'),
        submitData: {
          "demand_type": 0,
          "demand_category": 0,
          "demand_title": "",
          "user_name": "",
          "phone": "",
          "original_cost": "",
          "current_price": "",
          "end_time_desc": dateFormat(new Date(), 'YYYY-MM-DD HH:mm:ss'),
          "description": ""
        },
        oldDescription: '',
        oldDemandTitle: '',
        placeholder: {
          required: '必填，',
          title: '请输入2-25个字',
          username: '请输入2-5个字',
          phone: '请输入手机号码',
          cost: '请输入报酬原价',
          price: '请输入报酬金额',
          description: '请输入500字以内描述'
        }
      }
    },
    computed: { //计算
    },
    props: { //继承
    },
    methods: { //方法
      dataFn(value) {
        function CompareDate(d1, d2) {
          return((new Date(d1.replace(/-/g, "\/"))) > (new Date(d2.replace(/-/g, "\/"))));
        }
        if(CompareDate(dateFormat(new Date(), 'YYYY-MM-DD HH:mm:ss'), value)) {
          this.submitData.end_time_desc = dateFormat(new Date(), 'YYYY-MM-DD HH:mm:ss');
          this.promptFn('', '结束时间需大于当前时间');
        }
      },
      numberFn(num) {
        var reg = new RegExp("^[0-9]+(.[0-9]{1,2})?$");
        if(!(reg.test(this.submitData[num]))) {
          this.submitData[num] = '';
          this.promptFn('', '格式不正确');
        } else {
          if(this.submitData[num] < 0.01 || this.submitData[num] > 999999.99) {
            this.submitData[num] = '';
            this.promptFn('', '价格超出限制');
          }
        }
      },
      selectType() {
        this.gx_show = true
        this.$router.push({
          path: 'release',
          query: {
            from: 'type'
          }
        })
      },
      selectCate() {
        this.gx_show2 = true
        this.$router.push({
          path: 'release',
          query: {
            from: 'cate'
          }
        })
      },
      goBack() {
        this.gx_show = this.gx_show2 = false;
        history.back();
      },
      //弹窗
      promptFn(title, content) {
        this.$vux.alert.show({
          title: title || '温馨提示',
          content: content
        });
        setTimeout(() => {
          this.$vux.alert.hide()
        }, 3000);
      },
      //名字验证
      nameFn() {
        if(this.submitData.user_name.length < 2 || this.submitData.user_name.length > 5) {
          this.submitData.user_name = null;
          this.promptFn('', this.placeholder.username);
        }
      },
      nameFn2() {
        if(this.submitData.demand_title.length < 2 || this.submitData.demand_title.length > 25) {
          // this.submitData.demand_title = null;
          this.promptFn('', this.placeholder.title);
        }
      },
      nameFnInput2() {
        if(this.submitData.demand_title.length <= 25) {
          this.oldDemandTitle = this.submitData.demand_title;
        } else {
          this.submitData.demand_title = this.oldDemandTitle;
          this.promptFn('', this.placeholder.title);
        }
      },
      //手机号验证
      checkPhone() {
        if(!(/^1[34578]\d{9}$/.test(this.submitData.phone))) {
          this.submitData.phone = null;
          this.promptFn('', '手机号码格式不正确');
        }
      },
      //请输入500字以内描述
      detailsFn() {
        if(this.submitData.description.length <= 500) {
          this.oldDescription = this.submitData.description;
        } else {
          this.submitData.description = this.oldDescription;
          this.promptFn('', this.placeholder.description);
        }
      },
      //提交
      submitFn() {
        var v_this = this;
        var sd = this.submitData;

        function CompareDate(d1, d2) {
          return((new Date(d1.replace(/-/g, "\/"))) > (new Date(d2.replace(/-/g, "\/"))));
        }
        if(CompareDate(dateFormat(new Date(), 'YYYY-MM-DD HH:mm:ss'), this.submitData.end_time_desc)) {
          this.submitData.end_time_desc = dateFormat(new Date(), 'YYYY-MM-DD HH:mm:ss');
          this.promptFn('', '结束时间需大于当前时间');
        } else {
          if(sd.demand_title && sd.user_name && sd.phone && sd.original_cost && sd.current_price && sd.description && (sd.demand_title.length >= 2 && sd.demand_title.length <= 25)) {
            sd.end_time_desc = dateFormat(sd.end_time_desc, 'YYYY-MM-DD HH:mm:ss');
            var _data = {
              token: localStorage.getItem('userToken'),
              data: sd,
            }
            $$.post("/api/wxdemand/saveDemand", _data, function(data) {
              console.log('提交后', data);
              if(data.status == '0') {
                v_this.$router.push('/usercenter/release');
              } else {
                alert(data.msg);
              }
            });
          } else {
            this.promptFn('', '信息填写不正确！');
          }
        }
      }
    },
    watch: {
      $route() {
        this.gx_show = this.$route.query.from == 'type' ? true : false
        this.gx_show2 = this.$route.query.from == 'cate' ? true : false
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="less">
  @import url("../../assets/styles/less");
  .issue {
    padding-bottom: 1.00rem;
    ._date {
      position: relative;
      i {
        width: 0.30rem;
        height: 0.32rem;
        background: url(./rl.png) no-repeat;
        background-size: 100% 100%;
        position: absolute;
        top: 0.30rem;
        right: 0;
        z-index: 1;
      }
      a {
        display: block;
        width: 100%;
        height: 100%;
        padding-left: 0.5em;
        padding-right: 0.5em;
        color: @color6;
        position: relative;
        z-index: 2;
      }
    }
    .name_a {
      ._input i {
        display: block;
        width: 0.16rem;
        height: 0.28rem;
        background: url(r.png) no-repeat;
        background-size: 100% 100%;
        float: right;
        margin-top: 0.36rem;
      }
      ._input span {
        float: right;
        .font32;
        .lineH3;
        color: @color6;
        margin-right: 0.24rem;
      }
    }
    .vux-popup-dialog.vux-popup-right {
      overflow: hidden;
    }
    .popup_w {
      .margin_bottom {
        &:after {
          content: '';
          display: block;
          height: 0.20rem;
        }
      }
      input {
        display: block;
        width: 6.30rem;
        height: 1.00rem;
        margin: 0 auto;
        margin-top: 0.40rem;
        border-radius: 0.20rem;
        line-height: 1.00rem;
        .font36;
        border: none;
        background: #15aafb;
        color: @colorf;
        letter-spacing: 0.2em;
      }
      ._input i {
        width: 0.42rem;
        height: 0.28rem;
        background: none;
      }
      .name_a.active {
        i {
          background: url(dh.png) no-repeat;
          background-size: 100% 100%;
        }
      }
    }
    .name_input_w,
    .name_input_w2,
    .name_textarea_w,
    .name_fileImage_w {
      padding-left: 0.32rem;
      padding-right: 0.32rem;
      border-top: 1px solid @SecondaryText;
      background-color: @colorf;
      &:after {
        content: '';
        display: block;
        margin-left: -0.32rem;
        margin-right: -0.32rem;
        height: 0.20rem;
        border-top: 1px solid @SecondaryText;
        background-color: #ebebeb;
      }
    }
    .name_input_w {
      border-top: none;
    }
    .name_textarea_w:after {
      display: none;
    }
    .name_input {
      display: flex;
      border-bottom: 1px solid @SecondaryText;
      &:last-of-type {
        border-bottom: none;
      }
      .name {
        display: inline-block;
        width: 7em;
        .font32;
        .lineH3;
      }
      ._input {
        flex: 1;
        height: @font32*3;
        line-height: @font32*3;
        .font28;
        [type="text"],
        [type="number"] {
          width: 100%;
          height: 100%;
          padding-left: 0.5em;
          padding-right: 0.5em;
          border: none;
          color: @color6;
        }
      }
    }
    .name_textarea {
      border-bottom: 1px solid @SecondaryText;
      &:last-of-type {
        border-bottom: none;
      }
      .name {
        display: inline-block;
        width: 7em;
        padding-right: 0.5em;
        .font32;
        .lineH3;
        margin-bottom: -0.5em;
      }
      ._textarea {
        display: block;
        width: 100%;
        height: @font32*6;
        textarea {
          width: 100%;
          height: 100%;
          padding-bottom: 0.5em;
          border: none;
          resize: none;
          .font28;
          .lineH1_5;
          color: @color6;
        }
      }
    }
    ._button {
      width: 100%;
      height: 1.0rem;
      line-height: 1.0rem;
      .font36;
      position: fixed;
      bottom: 0;
      left: 0;
      border: none;
      background: #15aafb;
      color: @colorf;
      letter-spacing: 0.2em;
      z-index: 10;
    }
  }
</style>