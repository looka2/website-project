<template>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css">
  <div id="app">
    <header>
      <h1>Vimp Music Player</h1>
   </header>
    <main>
    <section class="player">
        <div class="image-banner">
        <img v-bind:src="current.banner" style="border-radius: 20px; width: 500px;">
          <br>
        </div>
        <br>
        <br>
        <h2 class="song-title">{{current.title}} - <span>{{current.artist}}</span></h2>
        <br>
        <div class="controls">
          <div class="center-buttons">
          <button class="prev" @click="prev"><svg xmlns="http://www.w3.org/2000/svg" height="2em" viewBox="0 0 512 512"><!--! Font Awesome Free 6.4.2 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2023 Fonticons, Inc. --><path d="M459.5 440.6c9.5 7.9 22.8 9.7 34.1 4.4s18.4-16.6 18.4-29V96c0-12.4-7.2-23.7-18.4-29s-24.5-3.6-34.1 4.4L288 214.3V256v41.7L459.5 440.6zM256 352V256 128 96c0-12.4-7.2-23.7-18.4-29s-24.5-3.6-34.1 4.4l-192 160C4.2 237.5 0 246.5 0 256s4.2 18.5 11.5 24.6l192 160c9.5 7.9 22.8 9.7 34.1 4.4s18.4-16.6 18.4-29V352z"/></svg></button>
          <button class="play" v-if="!isPlaying" @click="play"><svg xmlns="http://www.w3.org/2000/svg" height="2em" viewBox="0 0 384 512"><!--! Font Awesome Free 6.4.2 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2023 Fonticons, Inc. --><path d="M73 39c-14.8-9.1-33.4-9.4-48.5-.9S0 62.6 0 80V432c0 17.4 9.4 33.4 24.5 41.9s33.7 8.1 48.5-.9L361 297c14.3-8.7 23-24.2 23-41s-8.7-32.2-23-41L73 39z"/></svg></button>
          <button class="pause" v-else @click="pause"><svg xmlns="http://www.w3.org/2000/svg" height="2em" viewBox="0 0 320 512"><!--! Font Awesome Free 6.4.2 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2023 Fonticons, Inc. --><path d="M48 64C21.5 64 0 85.5 0 112V400c0 26.5 21.5 48 48 48H80c26.5 0 48-21.5 48-48V112c0-26.5-21.5-48-48-48H48zm192 0c-26.5 0-48 21.5-48 48V400c0 26.5 21.5 48 48 48h32c26.5 0 48-21.5 48-48V112c0-26.5-21.5-48-48-48H240z"/></svg></button>
          <button class="next" @click="next"><svg xmlns="http://www.w3.org/2000/svg" height="2em" viewBox="0 0 512 512"><!--! Font Awesome Free 6.4.2 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2023 Fonticons, Inc. --><path d="M52.5 440.6c-9.5 7.9-22.8 9.7-34.1 4.4S0 428.4 0 416V96C0 83.6 7.2 72.3 18.4 67s24.5-3.6 34.1 4.4L224 214.3V256v41.7L52.5 440.6zM256 352V256 128 96c0-12.4 7.2-23.7 18.4-29s24.5-3.6 34.1 4.4l192 160c7.3 6.1 11.5 15.1 11.5 24.6s-4.2 18.5-11.5 24.6l-192 160c-9.5 7.9-22.8 9.7-34.1 4.4s-18.4-16.6-18.4-29V352z"/></svg></button>
          </div>
          </div>

    </section>
      <section class="playlist">
        <br>
        <br>
        <br>
        <h3>Playlist</h3>
        <br>
        <br>
        <div class="controls-play">
        <button v-for="song in songs" :key="song.src" @click="play(song)" :class="(song.src == current.src) ? 'song playing' : 'song'" class="playlist-controls">
            {{song.title}} - {{song.artist}} <br />
          </button>
        </div>
      </section>
  </main>
  </div>
</template>

<script>

export default {
  name: 'App',
  data: function () {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      songs: [
        {
          title: 'Boarding song',
          artist: 'Emirates',
          src: require('./assets/Emirates - Boarding Song (Full).mp3'),
          banner: require('@/assets/E-logo.png')
        },
        {
          title: 'Piano Beat',
          artist: 'Unknown',
          src: require('./assets/Upbeat Piano Loop.mp3')
        },
        {
          title : 'Allahuakbar creeper',
          artist : 'A Youtube User',
          src: require('./assets/cursed.mp3')
        }
      ],
      player: new Audio()

    } 
  },
  methods: {
    play (song) {
      if (typeof song.src != "undefined") {
        this.current = song;
        this.player.src = this.current.src;
      }
      this.player.play();
      this.player.addEventListener('ended', function () {
        this.index++;
        if (this.index > this.songs.length - 1) {
          this.index = 0;
        }

        this.current = this.songs[this.index];
        this.play(this.current);
      }.bind(this));
      this.isPlaying = true;
    },
    pause () {
      this.player.pause();
      this.isPlaying = false;

    },
    next () {
      this.index++;
      if (this.index > this.songs.length -1) {
        this.index = -1;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    }, 
    prev () {
      this.index--;
      if (this.index < 0) {
        this.index = this.songs.length -1;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    }
   },
  created () {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
    //this.player.play();
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
  backgroud-color: #212121;
  background-color: #CBCBCB;
}
main {
  width: 100%;
  max-width: 768px;
  margin: 0 auto;
  padding: 25px;
}
.image-banner {
  display: flex;
  justify-content: center;
  margin-left: auto;
  margin-right: auto;
}
.song-title {
  color: #212121;
  font-size: 40px;
  font-weight: 700;
  text-transform: upercase;
  text-align: center;

}
.song-title span {
  font-weight: 400;
  font-style: italic;
}

.center-buttons {
  display: felx;
  justify-content: center;
  margin-left: auto;
  margin-right: auto;
  text-align: center;
}

.controls-play {
  display: block;
  align-items: center;
  justify-content: center;
}
.playlist-controls {
  border-radius: 10px;
  position: static;
  width: 100%;
  height: 80px;
  margin: 10px;
  border: 10px;
  background-color: #CBCBCB;
  
}

.prev, .play, .pause, .next {
  border-radius: 100%;
  text-align: center;
  border: 0;
  width: 50px;
  height: 50px;
  background-color: #FFF;

}

.playlist .song:hover {
  color: #FFF;
}
.playlist .song.playing {
  background-color: #A6A6A6;
}
h3 {
  font-size: 35px;
  text-align: center;
}


</style>
