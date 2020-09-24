<template>
  <div>
    <header class="header">
    <h1>Hello World</h1>
    </header>
    <main>
      <section class="player">
        <h2>{{ current.title }} -  <span>{{ current.artist }}</span></h2>
        <div class="controls">
          <button @click="previous">Previous</button>
          <button v-if="!isPlaying" @click="play">Play</button>
          <button v-else @click="pause">Pause</button>
          <button @click="next">Next</button>
        </div>
      </section>
      <section>
        <h3>My Playlist</h3>
        <button v-for="song in songs" :key="song.src" @click="play(song)" :class="(song.src == current.src) ? 'song-.playing' : 'song'">
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
      isPlaying: false,
      index: 0,
      songs: [
        {
          title: 'Mil Pasos',
          artist: 'Soha',
          src: require('./assets/Soha - Mil Pasos.mp3')
        },
        {
          title: 'Watermelon Sugar High',
          artist: 'Harry Styles',
          src: require('./assets/Harry Styles - Watermelon Sugar (Lyrics).mp3')
        },
        {
          title: 'Nice To Meet Ya',
          artist: 'Niall Horan',
          src: require('./assets/Niall Horan - Nice To Meet Ya (Lyrics).mp3')
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
    pause() {
      this.player.pause();
      this.isPlaying = false;
    },
    previous () {
      this.index--;
      if (this.index < 0) {
        this.index = this.songs.length - 1;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
    next () {
      this.index++;
      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    }
  },
  created () {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
  }
}
</script>

<style>
header {
  background-color: #ebc3e0;
  padding: 50px;
  margin-top: -55px;
}

</style>
