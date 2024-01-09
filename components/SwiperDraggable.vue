<!-- Please remove this file from your project -->
<template>
  <div class="wrapper">
    <h1>Swiper Draggable</h1>
    <div class="m-swiper">
      <div class="swiper-container">
        <div class="swiper-wrapper">
          <div v-for="(item, index) in swiperData" :key="index" class="swiper-slide">
            <div class="info-img" :style="`background:${item.color}`">
              <div class="title" v-text="item.text" />
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { Swiper, Autoplay, EffectCoverflow, Pagination, Navigation } from 'swiper'
import 'swiper/swiper.min.css'
Swiper.use(Autoplay)
Swiper.use(EffectCoverflow)
Swiper.use(Pagination)
Swiper.use(Navigation)

export default {
  name: 'SwiperDraggable',
  data () {
    return {
      swiper: null,
      slideOption: {
        effect: 'slide',
        loop: false,
        direction: 'horizontal',
        centeredSlides: false,
        slidesPerView: 1,
        spaceBetween: 20
        // slidesPerGroup: this.slidesPerGroup,
        // slidesOffsetBefore: this.onlyOne ? 0 : this.slidesOffsetBefore,
        // slidesOffsetAfter: this.onlyOne ? 0 : this.slidesOffsetAfter,
        // spaceBetween: this.onlyOne ? 0 : this.spaceBetween,
        // coverflowEffect: this.coverflowEffect
      },
      swiperData: [
        {
          color: '#ff0000',
          text: 'First slider'
        },
        {
          color: '#00ff00',
          text: '2nd slider'
        },
        {
          color: '#ff00ff',
          text: '3rd slider'
        },
        {
          color: '#ffff00',
          text: '4th slider'
        },
        {
          color: '#0000ff',
          text: '5th slider'
        }

      ]
    }
  },
  mounted () {
    this.createInstance()
  },
  methods: {
    createInstance () {
      this.swiper = new Swiper('.m-swiper .swiper-container', this.slideOption)
      setTimeout(() => {
        if (this.swiper) { this.setEffect(this.swiper) }
      }, 200)
    },
    setEffect (swiperTarget) {
      swiperTarget.on('setTranslate', (swiper, translate) => {
        const slideWidth = swiper.width + 20
        const delta = (Math.abs(translate) - swiper.activeIndex * slideWidth) / slideWidth
        let value = Math.round(delta * 100) / 100

        const nexttextobj = document.querySelector('.swiper-slide-next .info-img .title')
        const prevtextobj = document.querySelector('.swiper-slide-prev .info-img .title')

        const startTime = 0.3 // start at 30% process

        if (Math.abs(value) > startTime) {
          if (value > 1) { value = 1 }

          if (value > 0) { value -= startTime } else { value += startTime }

          if (nexttextobj) {
            nexttextobj.style.opacity = value
            nexttextobj.style.transform = `scale(${0.5 + (value > 0.5 ? 0.5 : value)})`
          }
          value = Math.abs(value)
          if (value > 1) { value = 1 }
          if (prevtextobj) {
            prevtextobj.style.opacity = value
            prevtextobj.style.transform = `scale(${0.5 + (value > 0.5 ? 0.5 : value)})`
          }
        }
      })

      swiperTarget.on('slideChangeTransitionEnd', (swiper) => {
        const nexttextobj = document.querySelector('.swiper-slide-next .info-img .title')
        const prevtextobj = document.querySelector('.swiper-slide-prev .info-img .title')
        if (nexttextobj) {
          nexttextobj.style.opacity = 0
          nexttextobj.style.transform = 'scale(0.5)'
        }
        if (prevtextobj) {
          prevtextobj.style.opacity = 0
          prevtextobj.style.transform = 'scale(0.5)'
        }
      })
    }
  }
}
</script>
<style lang="scss">
.wrapper{
  padding:20px;
  h1{
    margin:0 0 20px;
    font-size:24px;
    color:#666;
    font-weight:700;
  }
  .m-swiper {
    height:300px;
    .swiper-slide{
     .info-img{
      padding:30px;
      .title{
        text-align:center;
        font-weight:700;
        font-size:20px;
        opacity:0;
        transform:scale(0.5);
      }
     }
     &.swiper-slide-active{
      .info-img{
        .title{
          opacity:1 !important;
          transform:scale(1)  !important;
          transition: all 0.3s;
        }}
     }
    }
  }
}
</style>
