<template>
    <nav>
        <div class="logoCont" @click="redirectToExternal"><img class="loadingStar" src="../assets/Sekem-Star-icon.png" :class="{'loadingStarShow' : !this.$parent.docsIsLoaded || !this.$parent.chartsIsLoaded || !this.$parent.chartIsLoaded}"><img class="logoImg" src="../assets/Sekem-Holding-Logo-1.png" alt="SEKEM LOGO"></div>
        <div class="searchBarCont">
            <form action="">
                <input type="search" v-on:focus="this.$parent.showSearch = true" v-on:focusout="this.$parent.showSearch = false"  required>
                <i class="fa fa-search"></i>
            </form>
        </div>
    </nav>
    
</template>

<script>

    export default {
    methods: {
        redirectToExternal() {
        window.location.href = 'https://www.sekem.com/en/index/';
        },

  }
}

</script>

<style scoped>

nav{
  z-index: 1;
  position: fixed;
  width: 100%;
  background-color: rgb(173, 216, 230, .8);
  box-sizing: border-box;
  min-height: 75px;
  padding-left: 3%;
  padding-right: 3%;
  display: flex;
  justify-content: space-between;
  box-shadow: rgba(99, 99, 99, 0.2) 0px 5px 20px 0px;
}
.logoCont{
  display: flex;
  margin-top: auto;
  margin-bottom: auto;
  /* width: 120px; */
  cursor: pointer;
}

.logoImg{
  filter: drop-shadow(2px 2px 2px rgba(255, 50, 50, .5));
  padding: 5px;
  width: 130px;
  box-sizing: border-box;
}
.loadingStar{
  filter: drop-shadow(0px 0px 5px rgba(255, 255, 255, .5));
  width: 0px;
  scale: 0;
  transition: .2s ease-in-out;
}
.loadingStarShow{
  width: 50px;
  scale: 1;
  margin-right: 5px;
  animation: loadingAnim 1s cubic-bezier(0.5, 0, 0.5, 1) infinite;
}
@keyframes loadingAnim {
    0% {
        transform: rotate(0deg);
    }
    100% {
        transform: rotate(360deg);
    }
}

form {
  position: relative;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  transition: all .5s;
  width: 50px;
  height: 50px;
  background: white;
  box-sizing: border-box;
  border-radius: 4px;
  border: 4px solid white;
  padding: 5px;
  overflow-y: hidden;
}

input {
  position: absolute;
  top: 0;
  left: 0;
  width: 0;
  height: 42.5px;
  line-height: 30px;
  outline: 0;
  border: 0;
  font-size: 1em;
  border-radius: 20px;
  padding: 0 20px;
}

.fa {
  box-sizing: border-box;
  padding: 10px;
  width: 42.5px;
  height: 42.5px;
  position: absolute;
  top: 0;
  right: 0;
  border-radius: 4px;
  color: #07051a;
  text-align: center;
  font-size: 1.2em;
  transition: all .5s;
}
form:focus-within,
form:hover,
form:valid{
  width: 200px;
  cursor: pointer;
}

form:hover input,
form:focus-within input,
form:valid input {
  width: 100%;
} 

form:hover .fa,
form:focus-within .fa,
form:valid .fa {
  background: #07051a;
  color: white;
}

button {
  display: none;
  position: absolute;
  top: 70px;
  bottom: 0;
  left: 0;
  right: 0;
  font-size: 20px;
  color: white;
  padding: 4px;
  min-height: max-content;
  background: transparent;
  outline: 0;
  border: 1px solid #303030;
  border-radius: 4px;
  text-align: center;
  width: 100%;
  cursor: pointer;
}

form:valid button {
  display: block;
}
</style>

<script>

</script>