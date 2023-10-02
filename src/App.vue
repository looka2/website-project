<template>
  <div id="app">
    <header>
      <h1>Vimp Music Player</h1>
   </header>
    <main>
    <section class="player">
        <h2 class="song-title">{{current.title}} - <span>{{current.artist}}</span></h2>
        <div class="controls">
          <button class="prev" @click="prev">Prev</button>
          <button class="play" v-if="!isPlaying" @click="play">Play</button>
          <button class="pause" v-else @click="pause">Pause</button>
          <button class="next" @click="next">Next</button>
        </div>

    </section>
      <section class="playlist">
        <h3>Playlist</h3>
        <button v-for="song in songs" :key="song.src" @click="play(song)" :class="(song.src == current.src) ? 'song playing' : 'song'">
          {{song.title}} - {{song.artist}}
        </button>
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
          src: require('./assets/Emirates - Boarding Song (Full).mp3')
        },
        {
          title: 'Piano Beat',
          artist: 'Unknown',
          src: require('./assets/Upbeat Piano Loop.mp3')
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
      this.isPlaying = true;
    },
    pause () {
      this.player.pause();
      this.isPlaying = false;

    },
    next () {
      this.index++;
      if (this.index > this.songs.length -1) {
        this.index = 0;
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

</style>
