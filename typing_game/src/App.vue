<template>
  <h1 id="game_slogan" v-if="!display" style="display: block">
    How fancy are your fingers?
  </h1>
  <div class="gameBar">
    <div class="timer" v-if="display" style="opacity: 1">
      {{ `Timer: ${count} sec` }}
    </div>
    <div class="score">
      Score: {{ score }}
    </div>
  </div>
  <div v-if="display" style="display: block">
    <span
      v-bind:class="{
        correct: word.correct,
        wrong: word.wrong,
        pending: word.pending,
      }"
      :key="word.position"
      v-for="word in fetchedText"
      >{{ word.text }} {{ "" }}</span
    >
  </div>
  <textarea
    name=""
    id=""
    cols="30"
    rows="10"
    @keyup.space="processInput($event)"
    @keyup.enter="submitProgress($event)"
    v-if="display"
    style="display: block"
  ></textarea>
  <button @click="fetchData(), runTimer()" id="startButton" v-if="!display" style="display: inline">Start game</button>
  <button @click="stopTimer()" id="stopButton" v-if="display" style="display: inline">Quit</button>
</template>

<script>
export default {
  data() {
    return {
      inputValue: "",
      index: 0,
      display: false,
      count: 0,
      fetchedText: [],
      poems: [
        "my_name_in_love_language.txt",
        "she_loves_me.txt",
        "when_I_find_you.txt",
      ],
    };
  },
  computed: {
    score(){
      return `${this.fetchedText.filter((word) => word.correct).length} / ${this.fetchedText.length}`
    }
  }
  ,
  methods: {
    // rand(){
    //   let randIndex =  Math.floor(Math.random() * this.poems.length) + 1

    // },
    async fetchData() {
      const response = await fetch("text/she_loves_me.txt");
      const data = await response.text();
      let obj = data
        .trim()
        // Include non space characters including \r or \n
        .split(/\s+/)
        .map((word, index) => {
          return {
            position: index,
            text: word,
            correct: false,
            wrong: false,
            pending: true,
            myTimer: "",
          };
        });
      return (this.fetchedText = obj);
    },
    runTimer() {
      this.display = true;
      this.index = 0
      this.myTimer = setInterval(() => {
        ++this.count;
      }, 1000);
    },
    stopTimer() {
      this.count = 0;
      this.index = 0;
      this.display = false;
      clearInterval(this.myTimer);
    },
    submitProgress(event){
      event.preventDefault();
      // Include non space characters including \r or \n
      this.inputValue = event.target.value.trim().split(/\s+/);

      //To avoid index error
      if (this.index >= this.fetchedText.length) {
        this.display = false;
        return;
      }
      if (
        this.fetchedText[this.index].text ===
        this.inputValue[this.inputValue.length - 1]
      ) {
        //correct answer
        this.fetchedText[this.index].correct = true;
        this.fetchedText[this.index].wrong = false;
        this.fetchedText[this.index].pending = false;
      } else {
        //wrong answer
        this.fetchedText[this.index].correct = false;
        this.fetchedText[this.index].wrong = true;
        this.fetchedText[this.index].pending = false;
      }
      
      ++this.index
      this.display = false;
    },
    processInput(event) {
      event.preventDefault();
      // Include non space characters including \r or \n
      this.inputValue = event.target.value.trim().split(/\s+/);

      if (this.inputValue === "") {
        return;
      }
      //To avoid index error
      if (this.index >= this.fetchedText.length) {
        this.display = false;
        return;
      }
      if (
        this.fetchedText[this.index].text ===
        this.inputValue[this.inputValue.length - 1]
      ) {
        //correct answer
        this.fetchedText[this.index].correct = true;
        this.fetchedText[this.index].wrong = false;
        this.fetchedText[this.index].pending = false;
      } else {
        //wrong answer
        this.fetchedText[this.index].correct = false;
        this.fetchedText[this.index].wrong = true;
        this.fetchedText[this.index].pending = false;
      }
      
      ++this.index
    },
  },
};
</script>

<!-- Using sass -->
<style lang="scss">
body {
  user-select: none;
  background-color: #80deee;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
#app {
  background-color: #f6fdff;
  max-width: 50%;
  border-radius: 8px;
  padding: 1rem;
  box-shadow: 0px 0px 10px #929292;

  button {
    font-weight: bold;
    color: #fff;
    cursor: pointer;
    background-color: #663399;
  }
  #game_slogan {
    text-align: center;
    margin: 0px;
    display: none;
  }
  .gameBar {
    display: flex;
    justify-content: space-between;
    margin-bottom: 4px;
    .timer{
      opacity: 0;
    }
    .score,
    .timer {
      background-color: #111;
      color: #fefefe;
      min-width: 20px;
      max-width: 100px;
      height: 100%;
      padding: 4px;
      border-radius: 4px;
    }
  }
  .pending {
    font-weight: bold;
  }
  .correct {
    font-weight: bold;
    color: blue;
  }
  .wrong {
    font-weight: bold;
    border-bottom: 2px dotted #ff0000;
  }
}

textarea {
  width: 99%;
  max-width: 800px;
  border-radius: 8px;
  display: none;

  &:focus {
    outline: 2px solid #03bcdd;
    border-radius: 0px;
  }
  &::placeholder {
    padding: 1rem;
    font-size: larger;
    color: #757272;
    margin: auto;
  }
  #startButton{
    display: none;
  }
  #stopButton{
    display: none;
  }
}
</style>