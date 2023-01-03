<template>
  <div class="wrapper">
    <div class="container">
      <Tinder ref="tinder" key-name="id" :queue.sync="queue" :offset-y="10" @submit="onSubmit">
        <template slot-scope="scope">
          <div class="pic-wrapper" :style="{
  'background-image': `url(https://cn.bing.com//th?id=OHR.${scope.data.id}_UHD.jpg&pid=hp&w=720&h=1280&rs=1&c=4&r=0)`
}">

            <div class="card-text">
              <h1>Hallo Welt</h1>Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor
              invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo
              dolores et ea rebum.
            </div>
          </div>
        </template>
      </Tinder>
      <div class="btns
    ">
        <img src="./assets/rewind.png" @click="decide('rewind')">
        <img src="./assets/nope.png" @click="decide('nope')">
        <img src="./assets/super-like.png" @click="decide('super')">
        <img src="./assets/like.png" @click="decide('like')">
        <img src="./assets/help.png" @click="decide('help')">
      </div>
    </div>
  </div>
</template>

<script>
import Tinder from "vue-tinder";
import source from "@/bing";

export default {
  name: "App",
  components: { Tinder },
  data: () => ({
    queue: [],
    offset: 0,
    history: []
  }),
  created() {
    this.mock();
  },
  methods: {
    mock(count = 5, append = true) {
      const list = [];
      for (let i = 0; i < count; i++) {
        list.push({ id: source[this.offset] });
        this.offset++;
      }
      if (append) {
        this.queue = this.queue.concat(list);
      } else {
        this.queue.unshift(...list);
      }
    },
    onSubmit({ item }) {
      if (this.queue.length < 3) {
       // this.mock();
      }
      this.history.push(item);
    },
    async decide(choice) {
      if (choice === "rewind") {
        if (this.history.length) {
          this.$refs.tinder.rewind([this.history.pop()]);
        }
      } else if (choice === "help") {
        window.open("https://shanlh.github.io/vue-tinder");
      } else {
        this.$refs.tinder.decide(choice);
      }
    }
  }
};
</script>

<style>
html,
body {
  height: 100%;
  font-family: Arial, Helvetica, sans-serif;
}

body {
  margin: 0;
  background-color: #efefef;
}

.wrapper {
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.container {
  height: 90%;
  max-height: 1800px;
  min-height: 600px;
  aspect-ratio: 9/16;
  display: flex;
  flex-direction: column;
}

.vue-tinder {
  height: 100%;
  width: 100%;
  margin-bottom: 40px;
  flex-grow: 1;
}

.btns {
  min-width: 300px;
  max-width: 355px;
  margin: 0 auto;
}

.pic-wrapper {
  width: 100%;
  height: 100%;
  background-size: cover;
  background-position: center;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
}

.card-text {
  color: white;
  height: 200px;
  padding: 0 25px;
  text-shadow: black 0px 0px 5px;
}

h1 {
  padding: 0 0 8px 0;
  margin: 0;
  font-size: 1.5rem;
}

.btns img {
  margin-right: 12px;
  box-shadow: 0 4px 9px rgba(0, 0, 0, 0.15);
  border-radius: 50%;
  cursor: pointer;
  -webkit-tap-highlight-color: transparent;
}

.btns img:nth-child(2n + 1) {
  width: 53px;
}

.btns img:nth-child(2n) {
  width: 65px;
}

.btns img:nth-last-child(1) {
  margin-right: 0;
}
</style>
