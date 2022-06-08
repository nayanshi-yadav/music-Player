<template>
  <div id="app">
    <header>
      <h1>My Music</h1>
    </header>
    <main>
      <section class="player"> 
        <h2 class="song-title"> {{current.title}} - <span>{{current.artist}}</span></h2>
        <div class="controls">
          <button class="prev" @click="prev">Prev</button>
          <button class="play" v-if="!isPlaying" @click="play">Play </button>
          <button class="pause" v-else @click="pause">Pause </button>
          <button class="next" @click="next">Next</button>
        </div>
      </section>
      <section class="playlist">
        <h3>The Playlist</h3>
        <button v-for="song in songs" :key="song.src" @click="play(song)" 
        :class="(song.src == current.src) ? 'song_playing' : 'song' ">
        {{song.title}}- {{song.artist}}</button>
      </section>
    </main>
  </div>
</template>

<script>


export default {
  name: 'App',
  data(){
    return{
      current:{
      },
      index:0,
      isPlaying:false,
      songs:[
         {
           title:'Attention',
           artist:'Charlie Puth',
          src:require('./assets/Charlie Puth - Attention (DawnFoxes.com).mp3')     
         },
         {
           title:'Duniya se tujhko churake',
           artist:'Satyajeet',
           src:require('./assets/duniya se tujhko churake.mp3')
         },
         {
           title:'Udd ja kale',
           artist:'Cover-untitled',
           src:require('./assets/udd ja kale.mp3')
         },
      ],
      player: new Audio()

      

    }
  },
  created(){
    this.current=this.songs[this.index];
    this.player.src=this.current.src;
    
  },
 methods:{
  play(song){
       if (typeof song.src!="undefined"){
         this.current=song;
         this.player.src=this.current.src;

       }
       this.player.play();
       this.player.addEventListener('ended',function(){
         this.index++;
         if (this.index>this.songs.length -1){
           this.index=0;
         }
         this.current=this.songs[this.index];
         this.play(this.current);
       }.bind(this));
       
       this.isPlaying=true;
  },
 pause(){
   this.player.pause();
   this.isPlaying=false;
 },
 next(){
    this.index++;
    if(this.index>this.songs.length-1){
      this.index=0;
    }
    this.current=this.songs[this.index];
    this.play(this.current);
 },
 prev(){
   this.index--;
   if(this.index<0){
     this.index=this.songs.length -1;

   }
    this.current=this.songs[this.index];
    this.play(this.current);

 }
}
}
</script>

<style>
*{
  margin :0px;
  padding: 0px;
  box-sizing: border-box;
}
body{
  font-family: sans-serif;
}
header{
  display:flex;
  justify-content: center;
  align-items: center;
  padding:15px;
  background-color: rgb(208, 164, 236);
  color:rgb(85, 11, 107);
}
main{
  width:100%;
  max-width:768px;
  margin:0 auto;
  padding:25px;

}
.song-title{
color: darkmagenta;
font-size: 32px;
font-weight:700;
text-transform: uppercase;
text-align:center;
}
.song-title span{
  font-weight:400;
  font-style:italic;
}
.controls{
  display: flex;
  justify-content: center;
  align-items: center;
  padding:30px 15px;
}
button{
  appearance: none;
  background:none;
  border:none;
  outline: none;
  cursor: pointer;
}
button:hover{
  opacity:0.8;
}
.play, .pause{
  font-size: 20px;
  font-weight:700;
  padding:15px 25px;
  margin:0px 15px;
  border-radius: 8px;
  color: rgb(0, 0, 0);
  background-color: blueviolet;
}
.next ,.prev{
  font-size: 16px;
  font-weight:700;
  padding:10px 20px;
  margin :0px 15px;
  border-radius: 6px;
  color:cornsilk;
  background-color: rgb(219, 24, 219);
}
.playlist{
  padding: 0px 30px;
}
.playlist h3{
  color:rgb(55, 8, 59);
  font-size:28px;
  font-weight:400;
  margin-bottom: 30px;
  text-align: center;
}
.playlist .song{
  display:block;
  width:100%;
  padding:15px;
  font-size:25px;
  font-weight:700;
  cursor:pointer;
}
.playlist .song :hover{
  color: rgb(125, 232, 247);
}
.playlist .song_playing {
   display:block;
  width:100%;
  padding:15px;
  font-size:25px;
  font-weight:700;
  cursor:pointer;
  color:rgb(99, 10, 91);
  background-image: linear-gradient(to right, rgb(151, 106, 173),rgb(180, 84, 137));
}
</style>
