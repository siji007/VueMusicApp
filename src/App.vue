<template>
  <div id="app" class="container">
    <h1 class="text-center titleMusic  bg-orange-light py-4"> 
      My Music
    
    </h1>
    <main>
      <section class="player">
        <h2 class="song-title mt-10">{{current.title}} - <span>{{current.artist}} </span></h2> 
        <div class="flex justify-between mt-10">
          <button class="btn btn-primary text-2xl" @click="prev">Prev</button>
          <button class="btn btn-success text-2xl" v-if="!isPlaying" @click="play">Play</button>
          <button class="btn btn-danger text-2xl" v-else @click="pause">Pause</button>
          <button class="btn btn-secondary text-2xl" @click="next">Next</button>
          <!-- <button class="btn btn-secondary">Next</button> -->
        </div>
      </section>
      <div class="playlist">
        <h3 class="mt-4">Playlist</h3>
        <button v-for="song in songs" :key="song" @click="play(song)" :class="(song.src == current.src) ? 'song playing' : 'song'" class="mt-10  mr-4 p-3 text-2xl">
          {{song.title}} - {{song.artist}}
        </button>
      </div>
    </main>
  </div>
</template>


<script>
export default {
  name: 'App',
  components: {
  
  },
  data() {
    return {
      current:{},
      index: 0,
      isPlaying:false,
      songs:[
        {
        title:'What Can I do',
        artist:'Tye Tribet',
        src: require('./music/Tye-Tribbett-What-Can-I-Do.mp3')
      },
      {
      title:'One Nation',
      artist:'Timi Dakolo',
      src: require('./music/Timi_Dakolo_-_Great_Nation_Mp3bullet.ng.mp3')
      },
      {
      title: 'The Difference',
      artist:'Dunsin Oyekan',
      src: require('./music/Dunsin_Oyekan_-_The_Difference.mp3')
      },
     
      ],
      player: new Audio() //this will trigger an htmlAudio Element
    }
  },

  created() {
      this.current = this.songs[this.index];
      this.player.src = this.current.src;
      // this.player.play();

    },
  methods:{
    play(song){
      if(typeof song.src != "undefined"){
        this.current = song;
        this.player.src = this.current.src;
      }

      this.player.play();
      this.player.addEventListener('ended', function(){ //this will control what will happen after the song has ended!!
        this.index++;

         if(this.index > this.songs.length){
        this.index = 0;
      }
      this.current = this.songs[this.index];
      this.play(this.current);

      }.bind(this));
      this.isPlaying = true;
    },
    pause(){
      this.player.pause();
      this.isPlaying = false;
    },
    prev(){
      this.index--;
      if(this.index < 0){
        this.index = this.songs.length - 1;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
    next(){
      this.index++;
      if(this.index > this.songs.length - 1){
        this.index = 0;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    }
  }
}
</script>

<style>
.titleMusic{
  font-size: 50px;
}
.song-title{
  text-transform:upperCase;
  font-weight: 700;
  font-size: 42px;
}
.song-title span{
  font-weight: 400;
  
  font-style: italic;
}
button{
  appearance: none;
  cursor: pointer;
}
button:hover{
  opacity: 0.8;
}
.playlist h3{
  font-weight: 600;
  font-size: 38px;
}
.playlist .song{

}
.playlist .song.playing{
  color:white;
  background-image: linear-gradient(to right, #CC2E50, #FF5858);
}
</style>
