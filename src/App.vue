<template>
  <section class='wrapper'>
    <header>{{title}}</header>
    <section class='layout'>
      <article class='layout-left'>
        <dl v-for="item in pics" class="image-model" :key="item.key">
            <dt>{{item.time}}</dt>
            <dd><img v-for="v in item.images" :src="v" @click="imgZoom" @error="imgError"></dd>
        </dl>
      </article>
      <article class='layout-right'>
        <h4 @click="videoSplice">{{videotext}}</h4>
        <video v-for="v in videodata" width="320" height="240" :key="v.key" controls="controls">
        <source :src="v.src" type="video/mp4">
        Your browser does not support the video tag.
        </video>
      </article>
    </section>
    <div class='mask' v-show="zoom"></div>
  </section>
</template>

<script>
import vedioList from '@/data/video'
import times from '@/data/times'
let tempVideoData = vedioList.slice()
export default {
  name: 'app',
  data () {
    return {
      title: '温洵',
      videotext: '换一些视频',
      zoom: false, // 点击放大图片的状态，ture为默认缩小，false为放大
      pics: [], // 图片数据,
      images: [],
      auth: '',
      videodata: []
    }
  },
  methods: {
    imgZoom (evt) {
      var target = evt.currentTarget
      if (!this.zoom) {
        target.className = 'zoom'
        this.zoom = true
      } else {
        target.className = ''
        this.zoom = false
      }
    },
    // 构建图片数据
    buidImageData () {
      var self = this
      var preImages = 'DSC'
      var startNumber = 320
      var endNumber = 500
      var preUrl = 'static/images/'
      var tempNumber = ''
      var result = []
      for (; startNumber < endNumber; startNumber++) {
        // 构建为5位数
        tempNumber = (Array(5).join(0) + startNumber).slice(-5)
        this.images.push(`${preUrl}${preImages}${tempNumber}.JPG`)
      }
      times.forEach(function (item, index) {
        result.push({
          key: index,
          time: item.time,
          images: self.images.splice(0, item.number)
        })
      })
      this.pics = result
    },
    imgError (evt) {
      var target = evt.currentTarget
      target.style.display = 'none'
    },
    videoSplice () {
      if (this.auth === '20170507') {
        if (tempVideoData.length) {
          this.videodata = tempVideoData.splice(0, 3)
        } else {
          alert('没视频了,重头开始')
          tempVideoData = vedioList.slice()
          this.videodata = tempVideoData.splice(0, 3)
        }
      }
    }
  },
  created () {
    (function (self) {
      var number = 0
      function validate () {
        if (self.auth === '20170507') {
          self.buidImageData()
          self.videoSplice()
        } else {
          self.auth = prompt('请输入密码')
          number++
          if (number < 3) {
            validate()
          } else {
            self.title = '无权访问！！'
            self.videotext = ''
          }
        }
      }
      validate()
    })(this)
  }
}
</script>

<style lang="less">
@import '~@/assets/style/all';
.wrapper{
  padding:1rem;
}
.layout{
  display:flex;
  align-items: flex-start;
}
.layout-right{
  h4{
    text-align:center;
    padding:5px;
    cursor:pointer;
    color:#999;
    font-weight:normal;
    border-radius:5px;
    border:1px solid #ddd;
    background-color:#f2f2f2;
    &:hover{
      background-color:#fbfbfb;
    }
    margin-bottom:0.5rem;
  }
}
header{
  padding-bottom:1rem;
}
.image-model{
  display:flex;
  margin-bottom:10px;
  dt{
    flex:0 0 2.5rem;
    position:relative;
    margin-right: 0.5rem;
  }
  dd{
    display:flex;
    flex-wrap:wrap;
    align-items: flex-start;
    img{
      max-width:45%;
      display:block;
      cursor:zoom-in;
      border:5px solid @border;
      box-shadow: 5px 5px 10px 3px  @box-shadow;
      border-radius:5px;
      margin-right:10px;
      margin-bottom:10px;
      &.zoom{
        position: fixed;
        z-index: 20;
        left: 0px;
        top: 0px;
        bottom: 0px;
        right: 0px;
        margin: auto;
        max-width: 100%;
        max-height: 100%;
        cursor:zoom-out;
      }
    }
  }
}
.mask{
  overflow:hidden;
  position:fixed;
  width:100%;
  height:100%;
  left:0;
  top:0;
  background-color: rgba(0,0,0,.3);
  z-index:10;
}

</style>
