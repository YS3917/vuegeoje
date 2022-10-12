<template>
    <section class="visual">
      <!-- Swiper -->
      <Swiper 
      :modules="modules"
      :autoplay ="{
        delay:1000,
        disableOnInteraction: false
      }"
      :loop = "true"
      :pagination = "{
        el: '.sw-visual-pg',
        clickable: 'true'
      }"
      :effect = "'fade'"
      @swiper= "visualSlide"
      class="sw-visual"
      >

      <SwiperSlide class="swiper-slide" v-for="(item, index) in visualData" :key="index">
        <a 
        :href="item.link" 
        :style="{
          backgroundImage:`url(${ resW ? item.imgurl : item.imgurl_mb })`,
          backgroundSize:'cover',
          backgroundPosition:'center'
          }"

          :data-bgmb="item.imgurl_mb"
          :data-bg="item.imgurl"
        >
        </a>
      </SwiperSlide>


        <!-- 슬라이더 페이지네이션 -->
        <div class="sw-visual-control">
          <div class="sw-visual-pg"></div>
          <button class="sw-visual-bt">
            <span class="material-icons-outlined"  @click="controlSlide">
              {{slideState}}
            </span>
          </button>
        </div>

      </Swiper>


    </section>
</template>

<script>
import {
  Swiper, 
  SwiperSlide
} from 'swiper/vue'
import 'swiper/css'
import {Autoplay, EffectFade,Pagination}from 'swiper'
import 'swiper/css/pagination'
import 'swiper/css/effect-fade'

import {computed, onMounted ,ref} from 'vue'
import {useStore} from 'vuex'

export default {
  components : {
    Swiper,
    SwiperSlide
  },


  setup() {

    // state의 visualData를 받아온다.
    const store = useStore();
    // getter 의 visualData computed로 받아온다.
    const visualData = computed( () => store.getters.visualData);
    // Style 바인딩을 이용한다. :style="내용"
    // Swiper 슬라이드를 저장하려는 용도
      const slide = ref(null)

      const visualSlide = (swiper) => {
        slide.value = swiper; 
         // 1번부터 실행
        slide.value.slideTo(1); 
      }

      const slideState = ref('pause');
      const controlSlide = () => {
        if(slide.value.autoplay.running){
          slide.value.autoplay.stop();
          slideState.value = 'play_arrow'
        }else{  
          slide.value.autoplay.start();
          slideState.value = 'pause'
        }
      }

      // 현재 1400보다 크면 true, 작으면 false
      const resW = ref(true);

      onMounted( () => {
        // checkWin 메서드 : resw 체크 하는 메서드 1400
        const checkWin = () => {
          // 윈도우 너비를 체크 함.
          let w = window.innerWidth;
          // 1400 같거나 작으면
          if(w <= 1400) {
            resW.value = false;            
          }else{
            // 1400 보다 크다
            resW.value = true;            
          }
          // console.log(w)
          // console.log(resW.value)
        }
        // 윈도우에 이벤트를 연결
        window.addEventListener('resize', checkWin);
        // $(window).on('resize',function(){})
        // $(window).resize(function(){})

        // 최초 한번실행
        checkWin();

      })


    return{
      modules:[Autoplay,EffectFade,Pagination],
      visualSlide,
      slideState,
      controlSlide,
      visualData,
      resW
    }
  }
}
</script>


<style>
  /* 비주얼 영역 */
  .visual {
    position: relative;
    display: block;
    height: 477px;
  }

  .sw-visual {
    width: 100%;
    height: 100%;
  }

  .sw-visual a {
    position: relative;
    display: block;
    width: 100%;
    height: 100%;
  }

  .vs-1 {
    background: url('@/assets/images/banner_1_20200915_0.jpg') no-repeat center;
    background-size: cover;
  }

  .vs-2 {
    background: url('@/assets/images/banner_2_20200915_0.jpg') no-repeat center;
    background-size: cover;
  }

  .vs-3 {
    background: url('@/assets/images/banner_4_20200915_0.jpg') no-repeat center;
    background-size: cover;
  }

  .sw-visual-control {
    position: absolute;
    left: 50%;
    transform: translateX(-50%);
    bottom: 50px;

    display: block;
    z-index: 9;
  }

  .sw-visual-pg .swiper-pagination-bullet {
    width: 12px;
    height: 12px;
    background-color: #fff;
    opacity: 0.5;
    margin-right: 10px;
  }

  .sw-visual-pg {
    display: inline-block;
    width: auto !important;
  }

  .sw-visual-pg .swiper-pagination-bullet-active {
    opacity: 1.0;
  }

  .sw-visual-bt {
    position: relative;
    display: inline-block;
    width: 23px;
    height: 22px;
    background: transparent;
    vertical-align: bottom;

    /* background: url('@/assets/images/pause.png') no-repeat center; */
    cursor: pointer;
    border: 0;

    font-family: 'Material Icons';
    color: #fff;
    font-size: 10px;
  }

  .sw-visual-bt-play {
    background: url('@/assets/images/play.png') no-repeat center;
  }

  /* visual PC 버전 */
  /* visual 반응형 버전 */
  @media all and (max-width: 1400px) {
    .visual {
      height: 50vw;
    }
  }
</style>