<template>
<div>
  <div class="div">
        <div class="hread">
           <img src="@/assets/log.png">
           <input type="text" placeholder="请输入搜索的歌名" v-model="query" @keydown.enter="searchMusic">

        </div>
        <div class="conter">
         <div class="left">
           <ul>
             <li v-for="(item , index) in musicList" :key="index">
               <a @click="playMusic(item.id)"><img src="@/assets/bf1.png"></a>
               <p>{{item.name}}</p>
               <a v-if="item.mvid!=0" @click="playMv(item.mvid)"><img src="@/assets/mv.png" style="display: inline-block;margin-left: 15px;height: 30px;"></a>
             </li>
           </ul>
         </div>
         <div class="right" :class="{playing:isPlaying}">
           <img :src="musicCover" >
         </div>
        </div>
        <div class="foot">
           <audio controls="controls" autoplay="autoplay" loop="loop" :src="musicurl" @play="play" @pause="pause" ></audio>
        </div>
  </div>
<!-- mv -->
<div v-show="isShow" class="video">
  <video :src="mvurl" controls="controls"></video>
 <div class="mask" @click="hide"></div>
</div>
 
</div>
    
</template>

<script>
export default {
  name: 'Music',
  data(){
    return{
      //搜索歌曲
      query:"",
      //歌曲列表
      musicList:[],
      //播放歌曲路径
      musicurl:"",
      //歌曲封面
      musicCover:"",
      //音乐播放状态
      isPlaying:false,
      //遮罩层的显示状态
      isShow:false,
      //mv地址
      mvurl:"",

    }
  },
  methods:{
    //搜索歌曲
    searchMusic:function(){
      var that = this ;
      this.$axios.get("https://autumnfish.cn/search?keywords="+this.query).then(function(response){
        // console.log(response);
        that.musicList=response.data.result.songs;
        // console.log(that.musicList[1].name);
      },function(err){})
    },
    //播放音乐
    playMusic:function(musicId){
      var that = this ;
       this.$axios.get("https://autumnfish.cn/song/url?id="+musicId).then(function(response){
        that.musicurl=response.data.data[0].url;
      },function(err){})

      //切换封面
       this.$axios.get("https://autumnfish.cn/song/detail?ids="+musicId).then(function(response){
        // console.log(response.data.songs[0].al.picUrl);
        that.musicCover=response.data.songs[0].al.picUrl;
      },function(err){})
    },
    //播放
  play:function(){
    console.log("play")
    this.isPlaying=true;
  },
    //暂停
  pause:function(){
     console.log("pause")
    this.isPlaying=false;
    },
   //播放mv
  playMv:function(mvid){
    var that=this;
     this.$axios.get("https://autumnfish.cn/mv/url?id="+mvid).then(function(response){
        console.log(response.data.data.url);
        that.isShow=true;
        that.mvurl=response.data.data.url;
      },function(err){})
  },
  hide:function(){
    this.isShow=false;
  }

  },
 
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
*{
  margin: 0;
  padding: 0;
}
.div{
  /* background-color: 91b7fd; */
  width: 800px; 
  position: absolute;
    transform: translate(-50%, -50%);
    left: 50%;
    top: 50%;
  border:rgb(145, 183, 253) 2px solid;
  border-radius: 10px;
  box-shadow: #ccc 8px 10px 20px;
  /* padding: 10px; */
}
.video video{
  position: absolute;
    transform: translate(-50%, -50%);
    left: 50%;
    top: 50%;
    z-index: 99;
    width: 800px;
    border: 1px solid black;
}
ul li{
  list-style-type: none;
}
/* 头部 */
.div .hread{
  width: 100%;
  height: 40px;
  background: rgb(145, 183, 253);
  border-radius: 6px 6px 0 0;
  padding: 10px 0;
}
.div .hread img{
  height: 40px;
  padding-left: 10px;
  /* text-align: left; */
}
.div .hread input{
  display: inline-block;
    float: right;
    height: 25px;
    border: none;
    opacity: 0.7;
    border-radius: 10px;
    padding: 0 10px;
    margin-right: 10px;
    margin: 8px;
}
/* 中间 */
.div .conter{
  width: 100%;
  height: 400px;
  /* background-image: url("../assets/bj.jpg");
  background-size: 100% 100%; */
}
.div .conter .left{
  float: left;
  width: 320px;
  height: 100%;
  border-right: 1px solid  rgb(145, 183, 253);
  overflow-y: scroll;
  /* background: rgb(95, 89, 63); */
}
.div .conter .left ul{
  /* background: #91b7fd;
    opacity: 0.3; */
    margin: 10px;
    padding: 0px 5px;
    height: 30px;
    /* border-bottom: #ccc 1px solid; */
}
.div .conter .left ul li{
  /* background: orchid; */
    height: 30px;
    display: flex;
    align-items: center;
    border-bottom: #ccc 1px solid;
    padding: 10px 0;
    

}
.div .conter .left ul li p{
  width: 215px;
  /* background: burlywood; */
  overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;
}
.div .conter .left ul li a img{
  height: 20px;
  padding:5px 5px 0 0;
}
.div .conter .right{
  float: right;
  width: 470px;
  height: 100%;
  position: relative;
  /* background: rgb(190, 171, 94); */
}
.div .conter .right img{
  display: inline-block;
    position: absolute;
    transform: translate(-50%, -50%);
    left: 50%;
    top: 50%;
    width: 200px;
  border-radius: 50%;
  /* border: 6px solid rgb(145, 183, 253); */
}
.div .conter .playing{
  /* animation-play-state: running; */
  transform: rotate(360deg);
animation: rotation 3s linear infinite;
-moz-animation: rotation 3s linear infinite;
-webkit-animation: rotation 3s linear infinite;
-o-animation: rotation 3s linear infinite;
}
@keyframes rotation{
from {-webkit-transform: rotate(0deg);}
to {-webkit-transform: rotate(360deg);}
}
    
/* 尾部 */
.div .foot{
  width: 100%;
  height: 40px;
  background:  rgb(145, 183, 253);
  border-radius:0 0 6px 6px ;
  padding: 10px 0;
  overflow: hidden;
}
.div .foot audio{
  width:827px;;
  height: 40px;
  margin-left: -13px;

}
.mask{
    width: 100%;
    height: 100%;
    /* background-color: chartreuse; */
    /* background-image: url("../assets/mv.jpg"); */
  /* background-size: 100% 100%; */
  background: white;
    position: absolute;
}
</style>
