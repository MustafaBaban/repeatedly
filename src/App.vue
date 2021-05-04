<template>
  <div id="app">
    <img alt="Vue logo" class="logo" src="/img/logo.svg" />
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <h2>Link the video you want to repeat</h2>
    <div class="search">
      <input
        type="text"
        placeholder="Youtube Link"
        v-model="search"
        class="search-input"
      />
      <button class="search-button" v-on:keyup.enter="getId" @click="getId()">Get Video</button>
    </div>
    <h4>-</h4>
    <youtube
      :video-id="videoId"
      :player-vars="playerVars"
      ref="youtube"
      @playing="playing"
      @ended="ended"
      @cued="ready"
    ></youtube>
    <div class="buttons">
      <!-- <h2 class="title">Times</h2> -->
      <div class="times">
        <div class="times-button" @click="subtract()">
          <img class="img" src="/img/minus.svg" alt="" />
        </div>
        <input type="text" class="number" :value="times" />
        <div class="time-button" @click="add()">
          <img class="img" src="/img/plus.svg" alt="" />
        </div>
      </div>

      <div class="times">
        <div class="times-button" @click="subtractTime()">
          <img class="img" src="/img/minus.svg" alt="" />
        </div>

        <input type="text" class="number" :value="format(repeatTime)" />

        <div class="time-button" @click="addTime()">
          <img class="img" src="/img/plus.svg" alt="" />
        </div>
      </div>
    </div>

    <!-- <button class="play-button" @click="playVideo">
      <img src="/img/play.svg" alt="" />
    </button> -->
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'

export default {
  data() {
    return {
      videoId: "d1YBv2mWll0",
      times: 5,
      time: 0,
      videoLength: 0,
      videoTitle: null,
      search: null,
      playerVars: {
        playlist: "d1YBv2mWll0",
        autoplay: 0,
        loop: 0,
      },
    };
  },
  name: "App",
  mounted() {
    console.log(this.videoTime);
    // this.setInfo();
    // console.log(this.format(1111032));
  },
  components: {
    // HelloWorld
  },
  // ...
  computed: {
    player() {
      return this.$refs.youtube.player;
    },
    videoTime() {
      return this.$refs.youtube.player.getDuration();
    },
    repeatTime() {
      return this.videoLength * this.times;
    },
    // videoTitle() {
    //   return this.$refs.youtube.player.getVideoData().title;
    // },
  },
  methods: {
    ready(){
      // this.setInfo();
      if(!this.times==0){

        this.playVideo();
      }
      
      this.player.getDuration().then((value) => {
        console.log(value);
        this.videoLength = value;
        this.time = value * this.times;
      });
      console.log("ready");
    },
    playing(){
      
      this.player.getDuration().then((value) => {
        console.log(value);
        this.videoLength = value;
        this.time = value * this.times;
      });
      console.log("playing");
    },

    setInfo() {
      // this.player.getVideoData().then((value) => {
      //   console.log(value);
      //   this.videoTitle = value;
      // });
      this.player.getDuration().then((value) => {
        console.log(value);
        this.videoLength = value;
        this.time = value * this.times;
      });
    },
    getId() {
      this.videoId = this.$youtube.getIdFromUrl(this.search);
      // this.player.getDuration().then((value) => {
      //   console.log(Math.floor( value ));
      //   this.videoLength = Math.floor( value );
      //   this.time = value * this.times;
      // });
      // this.setInfo();
      // return this.$youtube.getIdFromUrl(this.search);
    },
    format(time) {
      // Hours, minutes and seconds
      var hrs = ~~(time / 3600);
      var mins = ~~((time % 3600) / 60);
      var secs = ~~time % 60;

      // Output like "1:01" or "4:03:59" or "123:03:59"
      var ret = "";
      if (hrs > 0) {
        ret += "" + hrs + ":" + (mins < 10 ? "0" : "");
      }
      ret += "" + mins + ":" + (secs < 10 ? "0" : "");
      ret += "" + secs;
      return ret;
    },

    ended() {
      if (!this.times <= 0) {
        this.times = this.times - 1;
        this.player.playVideo();
      }
      if (this.times == 0) {
        this.player.stopVideo();
      }
    },
    async playVideo() {
   
      await this.player.playVideo();
        this.player.getDuration().then((value) => {
        console.log(value);
        this.videoLength = value;
        this.time = value * this.times;
      });
      // Do something after the playVideo command
    },

    add() {
      this.times = this.times + 1;
      // let duration = 0;
      // duration = 0 ;
      this.player.getDuration().then((value) => {
        this.videoLength = value;
        this.time = this.times * this.videoLength;
      });

      // this.times = this.times * this.getDuration();
    },
    addTime() {
      this.time = this.time + this.videoLength * 2;
      this.times = this.times + 2;
    },
    subtract() {
      if (!this.times <= 0) {
        this.times = this.times - 1;
      }
    },
    subtractTime() {
      if (this.times > 0) {
      this.times = this.times - 2;
      }
    },
  },
};
</script>

<style >
html {
  background-color: #f8f8f8;
}
iframe {
  width: 100%;
  /* height: 50vw; */
  max-width: 650px; /* Also helpful. Optional. */
}

* {
  color: #20802f;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  margin-top: 30px;
  margin-left: 30px;
  margin-right: 30px;
}
.search-input {
  width: 80%;
  height: 40px;
  padding: 0;
  border-radius: 4px 0px 0px 4px;
  padding-left: 1em;
  border: 0;
  outline: none;
}
.search-button {
  width: 20%;
  color: white;
  border: 0;
  outline: none;
  font-weight: 600;
  border-radius: 0px 4px 4px 0px;
  cursor: pointer;
  background-color: #bd42ab;
}
.logo {
  width: 140px;
  margin-bottom: 20px;
}
.search {
  display: flex;
  width: 99%;
  height: 40px;
  background-color: white;
  border: #e4e3e3 solid 1px;
  border-radius: 4px;
}
::placeholder {
  color: #b8b3b3;
  /* padding-left: 1em; */
  /* font-size: 1.5em; */
}
.buttons {
  width: 100%;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-top: 2em;
}
.times {
  box-shadow: 0px 3px 5px rgba(0, 0, 0, 0.1);
  margin-bottom: 1em;
  justify-content: space-between;
  display: flex;
  /* margin-top:2em; */
  background-color: white;
  /* width: 48%; */
  height: 40px;
  border-radius: 4px;
}
.times-button {
  display: flex;
  background-color: #c4eac5;
  height: 30px;
  width: 30px;
  align-self: center;
  border-radius: 6px;
  margin-left: 10%;
  cursor: pointer;
}
.time-button {
  display: flex;
  background-color: #c4eac5;
  height: 30px;
  width: 30px;
  align-self: center;
  border-radius: 6px;
  margin-right: 10%;
  cursor: pointer;
}
.number {
  margin: 0;
  align-self: center;
  font-weight: 600;
  border: 0;
  outline: 0;
  width: 50%;
  font-size: 18px;
  text-align: center;
}
.img {
  width: 12px;
  margin: auto;
}
.play-button {
  display: flex;
  cursor: pointer;
  align-items: center;
  place-content: center;
  margin-top: 2em;
  width: 100%;
  outline: none;
  border: none;
  background-color: #bd42ab;
  height: 40px;
  color: white;
  border-radius: 3px;
}
.title {
  text-align: center;
}
@media (min-width: 768px) {
  #app{
    width:768px;
    padding-top:2em;
    margin:auto;
  }
  iframe {
    width: 100%;
    /* height: 50vw; */
    max-width: 768px; /* Also helpful. Optional. */
  }
  .buttons{
    flex-direction: row;
  }
  .times{
    width:48%;
  }
  .times-button{
    margin-left:20%;
  }
  .time-button{
    margin-right:20%;
  }
}

</style>
