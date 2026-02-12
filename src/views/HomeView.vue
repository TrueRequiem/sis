<template>

  <div class="gridCont">
    <NavComp></NavComp>
    <div></div>
    <OrgChart v-if="chartsIsLoaded" class="compDefaults" :class="{'showElement' : !showChart, 'notLoaded' : !chartsIsLoaded}"></OrgChart>
    <FileViewer v-if="docsIsLoaded" class="compDefaults" :class="{'showElement' : showDocs, 'notLoaded' : !docsIsLoaded}"></FileViewer>
  </div>

  <div class="pageBackground"></div>
  <div class="pageBackground gradient"></div>

   <div class="searchCont" :class="{'showElementBasic' : showSearch}">
      <div class="searchLoading">
        <div class="searchLoadingEl1 gradientAnimation"></div>
        <div class="searchLoadingEl2 gradientAnimation"></div>
        <div class="searchLoadingEl3 gradientAnimation"></div>
      </div>
      <div class="searchLoading">
        <div class="searchLoadingEl2 gradientAnimation"></div>
        <div class="searchLoadingEl3 gradientAnimation"></div>
        <div class="searchLoadingEl1 gradientAnimation"></div>
      </div>
      <div class="searchLoading">
        <div class="searchLoadingEl3 gradientAnimation"></div>
        <div class="searchLoadingEl2 gradientAnimation"></div>
        <div class="searchLoadingEl1 gradientAnimation"></div>
      </div>
    </div>

  <div class="ListCont" ref="ListRef"><ListHierarchy class="hoverHide"></ListHierarchy></div>
</template>

<script>
// @ is an alias to /src
import OrgChart from '@/components/OrgChartComp.vue'
import NavComp from '@/components/NavComp.vue';
import ListHierarchy from '@/components/ListHierarchyComp.vue';
import FileViewer from '@/components/FileViewerComp.vue';
import { ref } from 'vue';

export default {
  name: 'HomeView',
  components: {
    OrgChart,
    NavComp,
    ListHierarchy,
    FileViewer
  },
  data(){
    return{
      showChart : ref(false),
      showDocs : ref(false),
      docsIsLoaded : ref(true),
      chartsIsLoaded : ref(true),
      chartIsLoaded : ref(true),
      showSearch : ref(false),
      docsJSON : ref([{'file_url' : '/private/files/Skill.xlsx'}, {'file_url' : '/private/files/whatever.pdf'}, {'file_url' : '/private/files/something.doc'}, {'file_url' : '/private/files/Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.'} ])

    };
  },
  methods:{
    reset(){
      this.showChart = false;
      this.showDocs = false;
    }
  },
  setup(){

  },
}
</script>

<style scoped>

    .gridCont{
      min-height: 100vh !important;
      max-height: 100vh !important;
      position: relative;
      box-sizing: border-box;
    }
    .ListCont{
        top: 80px;
        height: 80vh;
        padding: 30px;
        box-sizing: border-box;
        width: 4vw;
        left: 0;
        scale: .8;
        transition: .3s ease-in-out;
        transition-timing-function: cubic-bezier(0.175, 0.885, 0.32, 1.275);
        transform-origin: left;
        z-index: 1;
        position: fixed;
        margin-bottom: 50px;
        backdrop-filter: blur(5px);
        -webkit-backdrop-filter: blur(5px);
        background-color: rgb(240,240,240, .5);
        background-image: url('../assets/pattern.png');
        background-size: 600px;
        opacity: 1;
        border-top-right-radius: 20px;
        border-bottom-right-radius: 20px;
        border: 1px solid rgb(0,0,0,.1);
    }
    .ListCont:hover{
        background-image: none;
        box-shadow: rgba(0, 0, 0, 0.35) 0px 5px 15px;
        background-color: rgba(240, 240, 240, 0.85);
        height: 89vh;
        top: 76px;
        width: auto;
        min-width: 30vw;
        opacity: 1;
        scale: 1;
        left: 0px;
    }
    .hoverHide{
        opacity: 0;
        transition: .2s ease-in-out;
    }
    .ListCont:hover > .hoverHide{
        opacity: 1;
    }
    .searchCont{
        z-index: 10;
        background-color: white;
        position: absolute;
        min-width: 250px;
        max-height: 300px;
        overflow-y: scroll;
        border-radius: 5px;
        padding: 15px;
        top: 90px;
        right: 3%;
        scale: 0;
        opacity: 0;
        transition: .3s;
        box-sizing: border-box;
        color: white;
    }
    .searchCont::-webkit-scrollbar{
        display: none;
    }
    .gradientAnimation {
        animation-duration: 1s;
        animation-fill-mode: forwards;
        animation-iteration-count: infinite;
        animation-name: Shimmer;
        animation-timing-function: linear;
        background: #f6f7f8;
        background: linear-gradient(to right, rgb(0,0,0,.2) 8%, rgb(0,0,0,.1) 38%, rgb(0,0,0,.2) 54%);
        background-size: 1000px 640px;
        position: relative;
    }
    @keyframes Shimmer{
        0%{
            background-position: -468px 0
        }
        100%{
            background-position: 468px 0
        }
    }
    .searchLoading{
      display: flex;
      justify-content: space-between;
      width: 100%;
      margin-bottom: 10px;
      margin-top: 10px;
    }
    .searchLoadingEl1{
      height: 5px;
      width: 10%;
      border-radius: 10px;
    }
    .searchLoadingEl2{
      height: 5px;
      width: 25%;
      border-radius: 10px;
    }
    .searchLoadingEl3{
      height: 5px;
      width: 60%;
      border-radius: 10px;
    }
    .pageBackground{
      z-index: -5;
      background-image: url("../assets/img5.jpg");
      background-size: cover;
      height: 100vh;
      width: 100vw;
      position: fixed;
      padding: 0;
      margin: 0;
      top: 0;
      left: 0;
      transform-origin: bottom;
    }
    .gradient{
      z-index: -4;
      background: RGBA(2, 0, 36, 0.50);
      background: linear-gradient(90deg, rgba(2, 0, 36, .5) 0%, rgba(255, 255, 255, .2) 35%, rgba(0, 212, 255, .2) 100%);
    }
    .hideElement{
      display: none;
    }
    .compDefaults{
      position: absolute;
      background-color: rgb(255, 255, 255, .90);
      border-bottom-left-radius: 5px;
      border-bottom-right-radius: 5px;
      border: 1px solid #999;
      scale: 0;
      opacity: 0;
      margin-left: 10px;
      margin-right: 10px;
      height: 88vh;
      box-sizing: border-box;
      transition: .3s ease-in-out;
      overflow-y: auto;
      overflow-x: auto;
    }
    .compDefaults::-webkit-scrollbar{
      width: .75em;
      height: .75em;
    }
    .compDefaults::-webkit-scrollbar-track{
      background: rgb(0, 0, 0,.1);
      border-radius: 10px;
      /* margin-block: 20px; */
      margin: 30px;
    }
    .compDefaults::-webkit-scrollbar-thumb{
      background: rgb(0, 0, 0,.1);
      border: 1px solid rgb(0, 0, 0, 0.05);
      cursor: pointer;
      border-radius: 10px;
    }
    .compDefaults::-webkit-scrollbar-thumb:hover{
      background: rgb(0, 0, 0,.15);
    }
    .showElement{
      position: relative;
      scale: 1;
      opacity: 1;
    }
    .notLoaded{
      pointer-events: none;
      filter: blur(5px);
    }
    .showElementBasic{
      scale: 1;
      opacity: 1;
    }

    @media screen and (max-height: 700px) {
      .compDefaults{
        height: 86vh;
      }
    }
    @media screen and (max-height: 600px) {
      .compDefaults{
        height: 84vh;
      }
    }
    @media screen and (max-height: 500px) {
      .compDefaults{
        height: 82vh;
      }
    }
</style>
