<template>
  <section class='wrapper'>
    <header>{{title}}</header>
    <section class='layout'>
      <article class='layout-left'>
        <dl v-for="(item, indexa) in imgdata" class="image-model" :key="indexa">
            <dt>{{item.time}}</dt>
            <dd><img v-for="(v, indexb) in item.images" :class="'img-'+indexa+'-'+indexb" :src="v" @click="imgZoom" @error="imgError"></dd>
        </dl>
      </article>
      <article class='layout-right'>
        <h4 @click="renderVideoAndImg">{{videotext}}</h4>
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
import images from '@/data/images'
let tempVideoData = vedioList.slice()
let tempimgdata = images.slice()
export default {
  name: 'app',
  data () {
    return {
      title: '温洵',
      videotext: '换一换',
      zoom: false, // 点击放大图片的状态，ture为默认缩小，false为放大
      imgdata: [], // 图片数据,
      videodata: [], // 视频数据
      auth: ''
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
    imgError (evt) {
      var target = evt.currentTarget
      target.style.display = 'none'
    },
    renderVideoAndImg () {
      if (this.auth === '20170507') {
        if (tempVideoData.length) {
          this.videodata = tempVideoData.splice(0, 3)
        } else {
          alert('没视频了,视频重头开始！')
          tempVideoData = vedioList.slice()
          this.videodata = tempVideoData.splice(0, 3)
        }
        if (tempimgdata.length) {
          this.imgdata = tempimgdata.splice(0, 3)
        } else {
          alert('没图片了,图片重头开始！')
          tempimgdata = vedioList.slice()
          this.imgdata = tempimgdata.splice(0, 3)
        }
      }
    }
  },
  created () {
    (function (self) {
      var number = 0
      function validate () {
        if (self.auth === '20170507') {
          self.renderVideoAndImg()
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
    margin: 0 auto;
    cursor:pointer;
    color:#999;
    width: 60/25rem;
    height: 60/25rem;
    line-height: 60/25rem;
    font-size: 14/25rem;
    font-weight:normal;
    border-radius:50%;
    box-shadow: 0 0 50px 3px rgba(0,0,0,.2);
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
    flex:0 0 90/25rem;
    font-size: 14/25rem;
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
