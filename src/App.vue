<template>
  <div id="app">
    <header>
      <h1>My Music</h1>
    </header>
    <main>
       
      <section class="player">
        <h2 class="song-title">{{ current.title }} - <span>{{ current.artist }}</span></h2>
       <!-- <audio controls id="audio" v-bind:src="current.src">
     
        </audio> -->
        <div id="audio"></div>
        <div class="controls">
          <button class="prev" v-on:click="prev()">Prev</button>
          <button class="play" v-if="!isPlaying" v-on:click="play(current)">Play</button>
          <button class="pause" v-else v-on:click="pause()">Pause</button>
          <button class="next" v-on:click="next()">Next</button>
        </div>
      </section>
      <section class="playlist">
        <h3>My Playlist</h3>
        <button v-for="song in songs" v-bind:key="song.src" v-on:click="play(song)" v-bind:class="(song.src == current.src) ? 'song playing' : 'song'">
          {{ song.title }} - {{ song.artist }}
        </button>
      </section>
    </main>
  </div>
</template>

<script>

export default {
  name: 'App',
  data () {
    return {
      current: {},
      index:0,
      isPlaying: false,
      songs: [
        {
          title: 'Tình Sầu Thiên Thu Muôn Lối',
          artist: 'Võ Đình Hiếu',
          src: require('./assets/TinhSauThienThuMuonLoi.mp3')
        },
         {
          title: 'Thich Thi Den',
          artist: 'Le Bao Binh',
          src: require('./assets/ThichThiDen.mp3')
        },
        {
          title: 'Cô Thắm Không Về',
          artist: 'Phát Hồ, Jokes Bii, Sinike',
          src: require('./assets/CoThamKhongVe.mp3')
        },
         {
          title: 'Bài Này Chill Phết',
          artist: 'Đen Vâu, MIN',
          src: require('./assets/BaiNayChillPhet.mp3')
        },
        {
          title: 'Mày Đang Giấu Cái Gì',
          artist: 'Andree, Đen',
          src: require('./assets/MayDangGiauCaiGi.mp3')
        },
         {
          title: 'Yêu Người Phản Bội',
          artist: 'Nhật Phong',
          src: require('./assets/YeuNguoiPhanBoi.mp3')
        }
      ],
      player: new Audio()
    }
  },
  methods: {
    play (song) {

      if(typeof song.src != "undefined" ){
        //dont change src when pause and play
        if(!document.getElementsByTagName('audio')[0].src.includes(song.src)){
          this.player.src = song.src;
        }
        this.current = song;
      }
      this.player.play();
      this.isPlaying = true;    
    },
    pause () {

      this.player.pause();
      //document.getElementsByTagName('audio')[0].pause();
      this.isPlaying = false;
    },
    prev () {
      let currentIndex = this.songs.indexOf(this.current);
      if(currentIndex <= 0){
        currentIndex = this.songs.length ;
      }
      let prevIndex = currentIndex -1;
      this.play(this.songs[prevIndex]);
    },
    next () {

      let currentIndex = this.songs.indexOf(this.current) + 1;
      if(currentIndex > (this.songs.length -1)){
        currentIndex = 0 ;
      }
      this.play(this.songs[currentIndex]);
    }
  },
  created () {
   
    this.current = this.songs[this.index];
    
    
  },
  mounted () {
    //this.player =  document.getElementById('audio');
    this.player =  document.createElement("AUDIO");
    this.player.setAttribute("controls", "controls");
    document.getElementById("audio").appendChild(this.player);

    this.player.addEventListener('ended', function(){
        let currentIndex = this.songs.indexOf(this.current) + 1;
      if(currentIndex > (this.songs.length -1)){
        currentIndex = 0 ;
      }
      this.current = this.songs[currentIndex];
      this.play(this.current);
    }.bind(this))
  }

  
}
 
</script>

<style>
* {
	margin: 0;
	padding: 0;
	box-sizing: border-box;
}
body {
	font-family: sans-serif;
}
header {
	display: flex;
	justify-content: center;
	align-items: center;
	padding: 15px;
	background-color: #212121;
	color: #FFF;
}
main {
  width: 100%;
  max-width: 768px;
  margin: 0 auto;
  padding: 25px;
}
.song-title {
  color: #53565A;
  font-size: 32px;
  font-weight: 700;
  text-transform: uppercase;
  text-align: center;
}
.song-title span {
  font-weight: 400;
  font-style: italic;
}
.controls {

  text-align:center;
  margin: 14px 15px ;
  display: flex;
  justify-content: center;
  align-items: center;

}
button {
  appearance: none;
  background: none;
  border: none;
  outline: none;
  cursor: pointer;
}
button:hover {
  opacity: 0.8;
}
.play, .pause {
  font-size: 20px;
  font-weight: 700;
  padding: 15px 25px;
  margin: 0px 15px;
  border-radius: 8px;
  color: #FFF;
  background-color: #CC2E5D;
}
.next, .prev {
  font-size: 16px;
  font-weight: 700;
  padding: 10px 20px;
  margin: 0px 15px;
  border-radius: 6px;
  color: #FFF;
  background-color: #FF5858;
}
.playlist {
  padding: 0px 30px;
}
.playlist h3 {
  color: #212121;
  font-size: 28px;
  font-weight: 400;
  margin-bottom: 30px;
  text-align: center;
}
.playlist .song {
  display: block;
  width: 100%;
  padding: 15px;
  font-size: 20px;
  font-weight: 700;
  cursor: pointer;
}
.playlist .song:hover {
  color: #FF5858;
}
.playlist .song.playing {
  color: #FFF;
  background-image: linear-gradient(to right, #CC2E5D, #FF5858);
}
audio {
  width: 95%;
  margin : 20px 0px 0px 20px;
}
</style>

