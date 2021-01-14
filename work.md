# 01-15

- [ ] 弹幕组件和购买提示组件分开,避免关闭弹幕后不显示
- [ ] 音视频上传 按下按钮背景颜色
- [ ] 去掉消息[删除]按钮
- [ ] 修改this.$confirm和this.$alert
- [ ] 更多栏图标permission的问题
- [ ] CLS(Cumulative Layout Shifts)分析

# 01-14

- [ ] ~~更多栏图标permission的问题~~
- [ ] ~~CLS(Cumulative Layout Shifts)分析~~
- [x] 讨论区问题的问字没居中
- [ ] 
- [x] 点击弹出清晰度切换弹窗
- [x] 语音直播底栏点击失效问题
- [x] 讨论区骨架屏改版
- [x] 录音层在取消录音后会消失
- [x] 苹果安全区需要重新修改
- [x] 更多按钮歪了



```css
// 苹果安全底部
.ios-safe-bottom{
  height: calc(constant(safe-area-inset-bottom) - 0.32rem);
  height: calc(env(safe-area-inset-bottom) - 0.32rem );
  width: 100%;
  background-color: #fff;
}
```



# 01-13

- [ ] ~~更多栏图标permission的问题~~
- [ ] ~~讨论区问题的问字没居中~~
- [x] 语音播放中状态 audioIsPlay
- [x] 语音遮罩无法关闭问题
- [x] 骨架屏抽到外面
- [x] 骨架屏动效
- [x] 骨架屏在首屏消息加载后消失
- [x] 首页更多按钮
- [ ] ~~CLS(Cumulative Layout Shifts)分析~~
- [x] InteractionSkeleton 重复加载问题
- [ ] ~~语音直播底栏点击失效问题~~
- [x] 回到顶部回到底部同时只存在一个
- [ ] ~~点击弹出清晰度切换弹窗~~

# 01-12

- [ ] CLS(Cumulative Layout Shifts)分析
- [ ] 更多栏图标permission的问题
- [x] wx_qr_url放入baseinfo
- [x] showRemoveMyCourseButton      solomonguo
- [x] 语音播放状态
- [x] badge中的数字不居中

- [ ] 讨论区问题的问字没居中
- [ ] 录音中

# 01-11

- [x] 体验报告的问题
- [x] 述职报告发送

- [ ] badge中的数字不居中

- [ ] 讨论区问题的问字没居中

- [x] 录音被讨论区遮挡
- [ ] CLS(Cumulative Layout Shifts)分析
- [ ] 更多栏图标permission的问题
- [ ] wx_qr_url放入baseinfo
- [ ] showRemoveMyCourseButton      solomonguo
- [ ] 语音播放状态



文本换行

```scss
overflow: hidden; //超出的文本隐藏
text-overflow: ellipsis; //溢出用省略号显示
white-space: nowrap; //溢出不换行
```



---

# 01-08

## 优化项

- 图片压缩,pandapng
- 打包加时间戳,与运维沟通
- 所有接口使用常量,统一到一个文件  const目录
- lib里面部分文件冗余,待优化
- 骨架到首屏时间太长
- 脚本报告,webpack打包大小分析
- vuex本地持久化
- 卡顿上报日志本地存储,下次打开再上报

### 待办事项

- [ ] 苹果底部安全区域按设计重新计算
- [x] 优惠券信息采集组件抽出
- [ ] 改写 Network.request  ajaxRequest
- [x] popover bug修复s

- [ ] 组件命名大驼峰

**广州花都区壹心培训中心有限公司**

直播日志卡上报

error_report_utils

# 01-07

- [x] 安卓返回键关闭pop操作
- [x] 录音vuex状态
- [x] 讨论区样式调整
- [x] alert error_tips 集体替换
- [x] 适配iphoex底栏

# 01-06

- [ ] 安卓返回键关闭pop操作
- [ ] 录音vuex状态
- [x] 顶部栏
- [x] 主题修复

# 01-05

- [x] 消息禁言,删除操作
- [x] 合并讨论区问题区

# 01-04

- [ ] 合并讨论区问题
- [x] 使用新的讨论区样式
- [x] 讨论区到顶部,到底部按钮
- [x] [上次看到这里]样式修改

---

# 12-31

- [x] 讨论区样式统一合并到style.scss

- [ ] 讨论区,问题区合并
- [x] 底栏功能以及接口

# 12-30

- [ ] 讨论区,问题区合并

- [x] 讨论区消息优化,wrapper文件

- [x] 商品更新问题
- [x] 底部栏新功能,对接口

# 12-29

**已完成**

底部栏morepanel

讨论区抽离分层,修复scroll滚动问题

# 12-28

**已完成**

1. 直播间2.0flex布局替换,以及讨论区scroll的问题
2. popup替换

**待完成**

1. 待完成 popup 返回键弹下
2. 底部栏morePanel

## Flex 滚动

```css
.wrap{
 display: flex;
  flex-direction: column;
  height: 100%;
  /* overflow:hidden; */
}

.header{
  height:80px;
  background-color:red;
}
.content{
  background-color:blue;
  flex-grow: 1;
  height:100%;
  overflow-y:scroll;
}

.footer{
  height:100px;
  background-color:gold;
}

.list{
  height:120px;
}

html,body{
  height:100%;
}
*{
  margin:0;
  padding:0;
}
```



## 12/22



直播间2.0 

远程一个客户,解决音质问题



![0f17f82079afa58ec1414d473887b90a](https://pic.4sus2.com/uPic/20201222225534KlONbI.jpg)

## 文字图片垂直居中

```scss
.company {
  text-align: center;
  font-size: 0.24rem;
  color: #cccccc;
  margin-bottom: 0.68rem;
  img {
    width: 0.24rem;
    height: 0.24rem;
  }
  img,
  span {
    vertical-align: middle;
  }
}
```

```js
<iframe frameborder="0" src="https://v.qq.com/txp/iframe/player.html?vid=i3074ov2fk3" allowFullScreen="true"></iframe>
```

https://i.y.qq.com/v8/playsong.html?songid=106345364&source=yqq#wechat_redirect

这里

https://v.qq.com/x/page/i3074ov2fk3.html



https_proxy=127.0.0.1:7890 brew cask reinstall font-hack-nerd-font

代理安装



```
#EXTM3U                         //m3u文件头，必须放在第一行
#EXT-X-VERSION:3                  
#EXT-X-MEDIA-SEQUENCE:2         //第一个TS分片的序列号  
#EXT-X-TARGETDURATION:5         //每个分片TS的最大的时长  
#EXT-X-ALLOW-CACHE:             //是否允许cache  
#EXT-X-ENDLIST                  //m3u8文件结束符 表示视频已经结束 有这个标志同时也说明当前流是一个非直播流
#EXT-X-PLAYLIST-TYPE:VOD/Live   //VOD表示当前视频流不是一个直播流，而是点播流(也就是视频的全部ts文件已经生成)
#EXTINF:                        //extra info，分片TS的信息，如时长，带宽等
```

# 1130

- 直播间2.0

  - 合并详情页和直播详情页
  - 直播间布局为tab栏模式

- 弹幕安卓优化

- 直播监控面板更新

- 工单处理


# 1125

需求:

- 监控面板接入数据
- 弹幕bug(过滤系统操作词)
- 紧急问题过程汇报

## 紧急问题过程汇报

商家在修改直播相关属性时出现`get alive from content platform error`报错.

### 出现问题商家:

appcw7jdmtg9894

appfy5ra0fm3037

app6dtxpo5l6738

appnlwpamfq1645

### 触发问题条件:

转播课程,删除了课程

### 问题原因:

素材中心和直播业务的资源状态没对齐

也正是因为重新接收，原来的接收的直播被商家删除了，直播id并不会变化，所以接收后也是删除状态。

是同一个id，那就是商家接收后，删除了，又重新接收了转播。

### 临时解决方案:

让商家在后台操作日志手动点击恢复课程能不能触发下数据同步，这边后续再排查逻辑问题

让商家在操作日志那里恢复

### 最终解决方案

接收转播的时候，把本地的直播表状态改为正常状态的同时，要把内容平台的状态也同步下。。

解决日期:

# 1124

需求:

- 直播监控面板
- 虚拟列表优化[边际判断,自动滚动,通用性修改]

[工单]

- 弹幕bug收敛

# 1118

- iphone7全屏卡
- 弹幕bug收敛
- 竖屏列表
- 360防录屏拓展



```bash
ffmpeg -i http://1252524126.vod2.myqcloud.com/9764a7a5vodtransgzp1252524126/702eb14c5285890809992499968/drm/v.f100230.m3u8  new.mp4
```



# 1117

- 新人培训
- 过长视频拼接失败
- 弹幕bug收敛

# 1116

## 需求

- 邀请达人榜
- 竖屏直播组件重写
- 弹幕bug收敛

## 工单

iphone无法观看





## 代码临时存放

```js
<template>
  <div class="comment-container" ref="comment"  @scroll="handleBarScroll">
    <div v-if="isLoading" class="loading">
      <span></span>
      <span></span>
      <span></span>
    </div>
    <div v-if="loadFailed">加载失败</div>
    <ul
      class="canTouchMove"
      ref="commentList"
    >
      <!-- 公告 -->
      <!-- <li 
        v-if="aliveNotice"
        class="comment-item"
        :key = "aliveNotice.msg_tag"
      >
        <span class="comment-avatar">{{aliveNotice.wx_nickname}}：</span>
        <span v-html="aliveNotice.msg_content"></span>
      </li> -->

      <!-- 历史消息 -->
      <li
        v-for="item of historyMsg"
        v-show="!basicMsg[0] || item.msg_tag != basicMsg[0].comment_id"
        :key="item.msg_tag"
        class="comment-item"
      >
        <p>
          <span class="comment-avatar">{{item.wx_nickname}}：</span>
          <span class="comment-msg">{{item.msg_content}}</span>
        </p>
      </li>
       <!-- 弹幕实时消息 odd -->
       <!-- <li style="height:0" class="infinite-list-phantom" ref="phantom"></li> -->
      <li
        v-for="item of visibleData"
        :key="item.index"
        :id="item.index"
        ref='items'
        :class="{'comment-item': true, 'basic-msg':true, 'noRead': item.item.noRead}"
      >
        <p>
          <span class="comment-avatar">{{item.item.wx_nickname}}：</span>
          <span class="comment-msg" v-if="!item.item.isNotice">{{item.item.msg_content}}</span>
          <span class="comment-msg" v-if="item.item.isNotice" v-html="item.item.msg_content"></span>
        </p>       
      </li>

    </ul>
  </div>
</template>

<script>
import EventBus from '~publicAsset/libs/eventbus';
import NetWork from '~publicAsset/libs/network';


export default {
  name: "vertical_msg_basic",
  props: ['room_info', 'isReplayClass', 'maxBasicMsgNum'],

  mounted() {

    let that = this;
    EventBus.$on('insert-alive-notice', (msg)=> {
      this.insertToBasicMsg({
        ...msg,
        is_show:true,
      });
      this.aliveNotice = msg;
      this.$nextTick(() => {
        this.canAutoScroll && this.autoScroll();
      });
    });
    EventBus.$on('cancel_mine_msg', this.deleteMsg);

    EventBus.$on('vertical-toggle-input', function() {
      console.warn('vertical-toggle-input');
      that.$nextTick(() => {
        that.canAutoScroll && that.autoScroll();
      });
    });
    this.$nextTick(()=>{
      if(this.isReplayClass){
        this.fetchHistoryMsg(0);
      }

    });
    this.$refs.comment.addEventListener('touchStart', this.touchStartHandler, { passive: false });
    this.$refs.comment.addEventListener('touchmove', this.touchMoveHandler, { passive: false });
    this.$refs.comment.addEventListener('touchend', this.touchEndHandler, { passive: false });

    // 虚拟列表实现
    //初始化
    this.screenHeight = this.$refs.comment.clientHeight;
    this.start = 0;
    this.end = this.start + this.visibleCount;
    let i = 0;
    // setInterval(() => {
    //     this.insertToBasicMsg({"msg_content":"我是"+i+++"条消息","content_type":0,"more_info":"","src_msg_content":"","src_nick_name":"","src_user_id":"","src_comment_id":"","src_type":0,"type":0,"is_show":1,"show_type":0,"is_can_exceptional":1,"user_type":1,"user_id":"u_5f9239a70e329_YuicqkgO50","wx_avatar":"http://wechatavator-1252524126.file.myqcloud.com/appAKLWLitn7978/image/compress/u_5f9239a70e329_YuicqkgO50.png","wx_nickname":"Yakir","user_title":"讲师","comment_id":"456df3e2bf37e5c61adc0856714f706d","app_id":"appAKLWLitn7978","alive_id":"l_5fa96037e4b0c3311f976530","msg_tag":Math.random()});
    // }, 1200);
    //  this.updateItemsSize();
    window.Test = ()=>{
    this.insertToBasicMsg({"msg_content":"我是第"+i+++"条消息","content_type":0,"more_info":"","src_msg_content":"","src_nick_name":"","src_user_id":"","src_comment_id":"","src_type":0,"type":0,"is_show":1,"show_type":0,"is_can_exceptional":1,"user_type":1,"user_id":"u_5f9239a70e329_YuicqkgO50","wx_avatar":"http://wechatavator-1252524126.file.myqcloud.com/appAKLWLitn7978/image/compress/u_5f9239a70e329_YuicqkgO50.png","wx_nickname":"Yakir","user_title":"讲师","comment_id":"456df3e2bf37e5c61adc0856714f706d","app_id":"appAKLWLitn7978","alive_id":"l_5fa96037e4b0c3311f976530","msg_tag":Math.random()});
    }

  },
  created(){
    //  this.initPositions();
  },
  computed:{
     // 给数据添加索引
    _listData() {
      // 如果消息超过2000条,则清空掉一半
      if(this.listData.length>2000){
       this.listData = this.listData.slice(0,1000);
      }
      return this.listData.map((item, index) => {
        return {
          _index: `_${index}`,
          item
        };
      });
    },
    visibleCount() {
      return Math.ceil(this.screenHeight / this.estimatedItemSize);
    },
    aboveCount() {
      return Math.min(
        this.start,
        Math.floor(this.bufferScale * this.visibleCount)
      );
    },
    belowCount() {
      return Math.min(
        this.listData.length - this.end,
        Math.floor(this.bufferScale * this.visibleCount)
      );
    },
    visibleData() {
      let start = this.start - this.aboveCount;
      let end = this.end + this.belowCount;
      return this._listData.slice(start, end);
    }
  },
   updated() {
    //  console.log('update',this.$refs.items,this.$refs.items.length);
    this.$nextTick(function() {
      // 如果数据不存在或者数据为空
      if (!this.$refs.items || !this.$refs.items.length) {
        return;
      }
      //获取真实元素大小，修改对应的尺寸缓存
      this.updateItemsSize();
      //更新列表总高度
      let height = this.positions[this.positions.length - 1].bottom;
      this.$refs.phantom.style.height = height + "px";
      //更新真实偏移量
      this.setStartOffset();
    });
  },
  methods: {
    addListData(msg){
      this.listData.push(msg);
      this.initPositions();
    },
    // 初始化位置信息
    initPositions() {
      let index = this.listData.length -1;
      this.positions.push({
        index,
        height: this.estimatedItemSize,
        top: index * this.estimatedItemSize,
        bottom: (index + 1) * this.estimatedItemSize
      });
      // this.positions = this.listData.map((d, index) => ({
      //   index,
      //   height: this.estimatedItemSize,
      //   top: index * this.estimatedItemSize,
      //   bottom: (index + 1) * this.estimatedItemSize
      // }));
    },
     //获取列表起始索引
    getStartIndex(scrollTop = 0) {
      //二分法查找
      // return this.binarySearch(this.positions,scrollTop);
      // if(this.positions.length===0){
      //   return ;
      // }
      // 原生查找法
      let item = this.positions.find(i => i && i.bottom > scrollTop);
      // 如果列表项目过少.
      if(item===undefined) {
        return 0;
      }
      return item.index;
    },
    //获取列表项的当前尺寸
    updateItemsSize() {
      let nodes = this.$refs.items;
      nodes.forEach(node => {
        let rect = node.getBoundingClientRect();
        // console.log(rect,node);
        let height = rect.height;
        let index = +node.id.slice(1);
        let oldHeight = this.positions[index].height;
        let dValue = oldHeight - height;
        //存在差值 就重新计算当前node的position
        if (dValue) {
          this.positions[index].bottom = this.positions[index].bottom - dValue;
          this.positions[index].height = height;
          // 将改节点以下的节点全部修改成最新的位置
          for (let k = index + 1; k < this.positions.length; k++) {
            this.positions[k].top = this.positions[k - 1].bottom;
            this.positions[k].bottom = this.positions[k].bottom - dValue;
          }
        }
      });
    },
    //获取当前的偏移量
    setStartOffset() {
      let startOffset;
      if (this.start >= 1) {
        let size =
          this.positions[this.start].top -
          (this.positions[this.start - this.aboveCount]
            ? this.positions[this.start - this.aboveCount].top
            : 0);
        startOffset = this.positions[this.start - 1].bottom - size;
      } else {
        startOffset = 0;
      }
      if(this.listData.length<=this.visibleCount){
        return;
      }
      this.$refs.commentList.style.transform = `translate3d(0,${startOffset}px,0)`;
    },
    //滚动事件
    scrollEvent() {
      //当前滚动位置
      let scrollTop = this.$refs.comment.scrollTop;
      console.log(scrollTop);
      // let startBottom = this.positions[this.start - ]
      //此时的开始索引
      this.start = this.getStartIndex(scrollTop);
      //此时的结束索引
      this.end = this.start + this.visibleCount;
      //此时的偏移量
      this.setStartOffset();
    },
    /**
     * 撤销消息
     */
    deleteMsg(msg) {
      let del_id = msg.comment_id;
      let delIndex = this.basicMsg.findIndex(item => item.comment_id === del_id);
      this.basicMsg.splice(delIndex, 1);
      this.basicMsg = [...this.basicMsg];
    },
    /**
     * 原index里的消息插入删除逻辑放到这里来写
     * 
     */
    insertToBasicMsg(msg) {
     
      if(msg.is_show || msg.user_id === this.room_info.user_id) {
        while(this.basicMsg.length > this.maxBasicMsgNum) {
          this.basicMsg.shift();
        }
        if(!this.canAutoScroll) { //如果当前是不能自动滚动的则 开始记录未读消息，标记为noRead= true
          msg.noRead = true;
          this.noLookMsgNum = this.noLookMsgNum + 1; //未读消息 +1
          EventBus.$emit('noLookMsgNumChange', this.noLookMsgNum); //通知父组件
        }
        this.basicMsg = [...this.basicMsg, msg];
        this.addListData(msg);
        this.$nextTick(() => {
          this.canAutoScroll && this.autoScroll() && this.msgAllRead(); //如果能自动滚动，就自动滚动，清除所有未读
        });
      }
    },
    handleBarScroll() { //滚动条scroll事件处理，兼容pc端滚动
      if(this.room_info.alive_status!==3){
         this.scrollEvent();
      }else{
          this.discussAreaDataController();
      }
     
    },  
    msgAllRead() { //所有已读处理
      this.canAutoScroll = true; //开启自动滑动
      this.noLookMsgNum = 0; //未读为0
      this.basicMsg.forEach( (element)=> {
        element.noRead = false;
      });
      EventBus.$emit('noLookMsgNumChange', 0);
    },
    dealHaveReadMsg() {
      let nowMsgs = document.getElementsByClassName('basic-msg');
      nowMsgs.forEach((element,index) => {
        if(this.isApearView(element)) {
          this.basicMsg[index].noRead = false;
        }
      });
      this.$nextTick(()=>{
        let nowMsgs = document.getElementsByClassName('noRead');
        this.noLookMsgNum = nowMsgs.length;
        EventBus.$emit('noLookMsgNumChange', this.noLookMsgNum);
      });
    },
    scrollHandle() {
      let scroll = this.$refs.comment;
      let scrollTop = scroll.scrollTop;
      let clientHeight = scroll.clientHeight;
      let scrollHeight = scroll.scrollHeight;
      //  只有回放拉取历史消息
      if(this.room_info.alive_status!==1){
        return;
      }
      //  拉取第一屏消息
      if ( ( scrollHeight - clientHeight ) - scrollTop < 0.3 && this.bottom_status === 1) {
        if (this.comment_id === '') {
          this.fetchHistoryMsg();
        } else {
          this.fetchHistoryMsg(1);
        }
      }
    },

    /**
     * loadTag为1时为 非第一屏加载
     */
    fetchHistoryMsg(loadTag) {
      
      let that = this;
      this.isLoading = true;
      let useCacheUrl = "_alive/get_more_msg";
      console.warn('消息', that.comment_id);

      let loadParms = {
        // 消息加载顺序（0为加载顶部消息，1为加载底部消息)
        loadOrder: loadTag ? 1 : 0,
        // 消息类型（0为加载讲师区消息，1为加载弹幕区，2为加载讨论区）
        infoType: 2,
        // 消息id
        comment_id: this.comment_id === '' ? '' : this.comment_id,
        // 读取历史消息
        loadHistory: 0,
        // 消息条数
        size: 20,
        // 直播id
        alive_id: this.room_info.alive_id,
        // 房间id
        room_id: this.room_info.room_id
      };
      let reqURL = useCacheUrl;

      NetWork.request(reqURL, loadParms, function(data) {
        if (data && data.code === 0) {
          let msgData = data.data.msgs;
          that.isLoading = false;
          if (msgData.length === 0) {
            that.bottom_status = 3;
            that.isLoading = false;
            return;
          }
          if (msgData.length < 19) {
            that.bottom_status = 3;
          } else {
            that.comment_id = msgData[msgData.length - 1].comment_id;
          }
          

          msgData = msgData.filter(item => item.content_type === 0);
          let msgs = msgData.map(item => that.changeMsgForm(item));
          that.historyMsg = that.historyMsg.concat(msgs);
          that.isLoading = false;
          that.$nextTick(() => {
            console.warn('消息', that.basicMsg);
            that.basicMsg[0] && that.canAutoScroll && that.scrollToBottom();
          });
        } else {
          that.loadFailed = true;
          that.isLoading = false;
        }
      });
    },

    changeMsgForm(msg) {
      let msgInfo = {
        wx_nickname: msg.wx_nickname,
        msg_content:  msg.org_msg_content,
        msg_tag: msg.comment_id
      };

      return msgInfo;
    },


    /* 
    * 滑动到底部
    */
    scrollToBottom() {
      // this.canAutoScroll = true;
      // let lastMsgDom = this.$refs.commentList.lastElementChild;
      // lastMsgDom && lastMsgDom.scrollIntoView();
    },

    autoScroll() {
      // let lastMsgDom = this.$refs.commentList.lastElementChild;
      // lastMsgDom && lastMsgDom.scrollIntoView();
    },

    touchStartHandler() {
      this.canAutoScroll = false;
      //  10s 之后滚动到最新消息
      window.setTimeout(() => {
        this.canAutoScroll = true;
        let lastMsgDom = this.$refs.commentList.lastElementChild;
        lastMsgDom && lastMsgDom.scrollIntoView();
      }, 10000);
    },
    discussAreaDataController(){
      // console.warn('scroll state', this.isLoading, this.bottom_status);
      this.controllAutoScroll();
      !this.canAutoScroll && this.dealHaveReadMsg();
      if (!this.isLoading && this.bottom_status !== 3) {
          this.scrollHandle();
      }
    },
    touchMoveHandler() {
      this.discussAreaDataController();
    },
    controllAutoScroll() {
      let scroll = this.$refs.comment;
      let scrollTop = scroll.scrollTop;
      let clientHeight = scroll.clientHeight;
      let scrollHeight = scroll.scrollHeight;
      if( (scrollHeight - clientHeight ) - scrollTop < 0.3) {
        this.canAutoScroll = true;
        this.msgAllRead();
      }else {
        this.canAutoScroll = false;
      }
    },

    isApearView(currentMsg){//判断一条消息是否在可视区域
      let scroll = this.$refs.comment;
      let scrollBcr = scroll.getBoundingClientRect();
      let scrollRectTop = scrollBcr.top;
      let scrollRectBottom = scrollBcr.top + scrollBcr.height;
      let msgBcr = currentMsg.getBoundingClientRect();
      let msgPosition = msgBcr.top;
      return msgPosition>scrollRectTop && msgPosition< scrollRectBottom;
    },
    touchEndHandler() {
      this.controllAutoScroll();
      // let scrollTop = scroll.scrollTop;
      // console.log(msgPosition, scrollRectTop, scrollRectBottom);
      // console.log();
    }
  },
  data() {
    return {
      basicMsg: [],
      historyMsg: [],
      aliveNotice: null,
      canAutoScroll: true, // 触摸状态下新消息不自动滚动
      bottom_status: 1, //顶部加载状态 区域是否可加载，1为可加载，2为加载中，3为加载完（没有数据了，4为加载失败
      isLoading: false, //  正在加载中
      loadFailed: false, //  加载失败
      comment_id: '',
      noLookMsgNum: 0 ,//没有看过的消息
      // 虚拟列表实现
      listData:[] ,// 所有数据,永久缓存
      screenHeight: 0,//可视区域高度
      start: 0, //起始索引
      end: 0,//结束索引
      estimatedItemSize:20,//预估大小
      bufferScale:1,// 缓冲区
      positions:[]
    };
  }
};
</script>

<style lang="scss" scoped>
.comment-container{
  font-size: 14px;
  color: #fff;
  width: 206px;
  height: 160px;
  overflow: scroll;
  display: inline-block;
  bottom: 0;
  position: absolute;
  -webkit-mask-image: -webkit-linear-gradient(top,transparent 0%,rgba(0,0,0,.1) 1%,rgba(0,0,0,.8) 7.2%,#fff 100%);
  .loading{
    >span{
      display: inline-block;
      width: 8px;
      height: 8px;
      border-radius: 4px;
      margin-right: 8px;
      &:nth-of-type(1) {
        background:rgba(0,0,0,0.6);
      }
      &:nth-of-type(2) {
        background:rgba(0,0,0,0.4);
      }
      &:nth-of-type(3) {
        background:rgba(0,0,0,0.2);
      }
    }
  }
  .comment-item{
    padding: 0px 8px;
    background: rgba(0,0,0,0.24);
    border-radius: 16px;
    width: 206px;
    box-sizing: border-box;
    position: relative;
    // margin-top: 4px;
    .comment-avatar{
      color: rgba(178, 208, 254, 1);
      font-weight: 600;
    }
    .comment-msg{
      word-break: break-all;
    }
  }
}
::-webkit-scrollbar{
  display:none;
}
.infinite-list-phantom {
  position: absolute;
  left: 0;
  top: 0;
  right: 0;
  z-index: -1;
}
</style>

```







# 1109

## 工单

- 商家表示某些安卓卡[可能是X5的原因]
- 商家表示学员Safari无法观看[看到没有完全兼容Safari]

## 需求

- 弹幕继续测试和优化
- 360防录屏还 未完成
- 双11竖屏直播弹幕优化

#1110

## bug

- aliveId->alive_id 

##  需求

- 邀请达人优化榜 明日对接
- 弹幕测试问题  [假录播换了播放器]
- 竖屏弹幕优化
  - 直播中不加载历史弹幕[OK]

## 直播中心月会

自己的目标是什么,追求是什么

客户中心 

信息传递 

规矩纪律

# 1111

## 需求

- 竖屏直播弹幕优化

## 工单

- 【【5G手机观看直播卡顿】宇宙商学院+app7j0E1TVn1769【商家直播一直出现手机观看卡顿问题，多个学员手机观看都显示卡顿，电脑测试是网速正常】】https://www.tapd.cn/23360971/bugtrace/bugs/view?bug_id=1123360971001196959

- 【【5G手机观看直播卡顿】乐客学院+appObRPQOZ35459【华为手机，5G手机看直播会时不时卡顿，反馈多次了（第三次反馈）】】https://www.tapd.cn/23360971/bugtrace/bugs/view?bug_id=1123360971001197236

# 1112

## 需求

- 360防止录屏[已提交合并]
- 邀请达人榜对接
- 弹幕上测试环境

## 工单 

- mac自带浏览器无法播放 已完成
- Iphone11 无法播放

# 1113

## [需求]

1、 360防录屏功能【已经完成】
2、邀请达人榜将界面对接到live-h5
3、弹幕功能上测试环境，bug收敛

## [工单]

1、帮用户解决Safari无法播放的问题【已经完成】
2、iphone11直播中转圈，无法播放