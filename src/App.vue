<template>
  <div id="app">
    <main>
      <header>
        <img src="./assets/logo.svg" class="img-fluid" alt="site logo">
        <h1>Music Player</h1>
      </header>
      <section class="player">
        <div class="song-card">
          <img :src="current.cover" class="img-fluid" alt="song cover" >
          <h2 class="song-title">
            {{ current.title }}
            <br>
            <span>{{ current.artist }}</span>
          </h2>
        </div>
        <div class="controls">
          <button class="prev" @click="prev"><i class="fas fa-step-backward"></i> Prev</button>
          <button class="play" v-if="!isPlaying" @click="play">Play <i class="far fa-play-circle"></i></button>
          <button class="pause" v-else @click="pause">Pause <i class="far fa-pause-circle"></i></button>
          <button class="next" @click="next">Next <i class="fas fa-step-forward"></i></button>
        </div>
      </section>
      <section class="playlist">
        <h3>The Playlist</h3>
        <button v-for="song in songs" :key="song.src" @click="play(song)" :class="(song.src == current.src) ? 'song playing' : 'song'">
          {{ song.title }} - {{ song.artist }}
        </button>
      </section>
    </main>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      songs: [
        {
          title: 'Circles',
          artist: 'Post Malone',
          src: require('./assets/music/circles.mp3'),
          cover: require('./assets/covers/circles.png')
        },
        {
          title: 'Paris',
          artist: 'The Chainsmokers',
          src: require('./assets/music/paris.mp3'),
          cover: require('./assets/covers/paris.png')
        },
        {
          title: 'Blinding Lights',
          artist: 'The Weeknd',
          src: require('./assets/music/blinding-lights.mp3'),
          cover: require('./assets/covers/blinding-lights.png')
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
        if (this.index > this.songs.length -1) {
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
      if (this.index > this.songs.length - 1) {
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
  }
}
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: sans-serif;
  background: #2f3136;
}


main {
  width: 100%;
  max-width: 768px;
  margin: 5vh auto;
  padding: 25px;
  border-radius: .75rem;
  box-shadow: 0 2px 10px 0 rgba(0,0,0,.2);
  background-color: #36393f;

  header {
    display: flex;
    justify-content: center;
    align-items: center;
    color: #fff;
    margin-bottom: 50px;

    img {
      margin-right: 20px;
    }
  }

  .player {
    .song-card {
      display: flex;
      justify-content: center;
      align-items: center;

      img {
        width: 256px;
        height: 256px;
        display: block;
        margin: 0 30px 0 0;
        border-radius: .50rem;
      }
      .song-title {
        color: #fff;
        font-size: 32px;
        font-weight: 700;
        text-transform: uppercase;

        span {
          font-weight: 400;
          font-style: italic;
          font-size: 70%;
        }
      }
    }
    .controls {
      display: flex;
      justify-content: center;
      align-items: center;
      padding: 30px 15px;

      button {
        appearance: none;
        background: none;
        border: none;
        outline: none;
        cursor: pointer;
        transition: .2s;

        &:hover {
          opacity: .8;
        }

        &.play,
        &.pause {
          font-size: 20px;
          font-weight: 700;
          padding: 15px 25px;
          margin: 0 15px;
          border-radius: 8px;
          color: #fff;
          background-color: #CC2E5D;
        }

        &.next,
        &.prev {
          font-size: 16px;
          font-weight: 700;
          padding: 10px 20px;
          margin: 0 15px;
          border-radius: 6px;
          color: #fff;
          background-color: #FF5858;
        }
      }
    }
  }
  .playlist {
    padding: 0 30px;
    margin-top: 50px;

    h3 {
      color: #fff;
      font-size: 28px;
      font-weight: 400;
      margin-bottom: 30px;
      text-align: center;
    }

    .song {
      appearance: none;
      background: none;
      border: none;
      outline: none;
      display: block;
      width: 100%;
      padding: 15px;
      font-size: 20px;
      font-weight: 700;
      cursor: pointer;
      color: #fff;

      &:hover {
        color: #FF5858;
      }

      &.playing {
        color: #FFF;
        border: none;
        background-image: linear-gradient(to right, #CC2E5D, #FF5858);
      }
    }
  }
}
</style>
