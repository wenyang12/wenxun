<template>
  <section class='wrapper'>
  <header>{{foo}}</header>
  <article>
    <dl v-for="item in pics" class="image-model">
        <dt>{{item.time}}</dt>
        <dd><img v-for="v in item.images" :src="v" @click="imgZoom" @error="imgError"></dd>
    </dl>
  </article>
  <div class='mask' v-show="zoom"></div>
  </section>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      foo: '温洵',
      zoom: false, // 点击放大图片的状态，ture为默认缩小，false为放大
      pics: [], // 图片数据,
      images: [],
      auth: ''
    }
  },
  created () {
    this.auth = prompt('请输入密码')
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
      var times = [{
        time: '2017-5-10',
        number: 6
      }, {
        time: '2017-5-11',
        number: 4
      }, {
        time: '2017-5-12',
        number: 6
      }, {
        time: '2017-5-13',
        number: 9
      }, {
        time: '2017-5-14',
        number: 1
      }, {
        time: '2017-5-15',
        number: 5
      }, {
        time: '2017-5-18',
        number: 7
      }, {
        time: '2017-5-19',
        number: 5
      }, {
        time: '2017-5-20',
        number: 1
      }]
      for (; startNumber < endNumber; startNumber++) {
        // 构建为5位数
        tempNumber = (Array(5).join(0) + startNumber).slice(-5)
        this.images.push(`${preUrl}${preImages}${tempNumber}.JPG`)
      }
      times.forEach(function (item) {
        result.push({
          time: item.time,
          images: self.images.splice(0, item.number)
        })
      })
      this.pics = result
    },
    imgError (evt) {
      var target = evt.currentTarget
      target.style.display = 'none'
    }
  },
  mounted () {
    (function (self) {
      var number = 0
      function validate () {
        if (self.auth === '20170507') {
          self.buidImageData()
        } else {
          alert('输入的密码不正确，请重新输入')
          self.auth = prompt('请输入密码')
          number++
          if (number < 3) {
            validate()
          } else {
            self.foo = '无权访问！！'
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
