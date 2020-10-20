<template>
  <h1>How fancy are your fingers?</h1>
  <div class="timer">{{ `${count} sec` }}</div>
  <div>
     <span v-bind:class = "{ 
          correct: word.correct,
          wrong: word.wrong,
          pending: word.pending,
      }"
      :key="word.position" 
      v-for="word in fetchedText">{{ word.text }} {{ '' }}</span>
  </div>
  <textarea
    name=""
    id=""
    cols="30"
    rows="10"
    @keyup.space="processInput($event)"
  ></textarea>
  <button @click="fetchData(), runTimer()">Click</button>
</template>

<script>
export default {
  data() {
    return {
      inputValue: "",
      index: 0,
      count: 0,
      fetchedText: [],
    };
  },
  methods: {
    async fetchData() {
      const response = await fetch("text/find_you.txt");
      const data = await response.text();
      let obj = data
        .trim()
        .split(" ")
        .map((word, index) => {
          return {
            position: index,
            text: word,
            correct: false,
            wrong: false,
            pending: true,
          };
        });
      return (this.fetchedText = obj);
    },
    runTimer(){
        setInterval(() => {
            ++this.count
        }, 1000)
    },
    processInput(event) {
      event.preventDefault();
      this.inputValue = event.target.value.trim().split(" ");
      
      console.log(this.inputValue.length)

      if (this.inputValue === "") {
        return;
      }
      if (this.fetchedText[this.index].text === this.inputValue[this.inputValue.length - 1]) {
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
      this.index++;
    },
  },
};
</script>

<style>
body{
  user-select: none;
  background-color: #80deee;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
#app{
  background-color: #f6fdff;
  max-width: 50%;
  border-radius: 8px;
  padding: 1rem;
  box-shadow: 0px 0px 10px #929292;
}
.timer {
  background-color: #111;
  color: #fefefe;
  min-width: 20px;
  max-width: 50px;
  height: 30px;
  display: flex;
  align-items: center;
  justify-content: space-evenly;
  padding: 2px;
  border-radius: 4px;
  /* opacity: 0; */
}
.pending {
    font-weight: bold;
}
.correct {
    font-weight: bold;
    color: green;
}
.wrong{
    font-weight: bold;
    color: red;
}
textarea {
  width: 99%;
  max-width: 800px;
  border-radius: 8px;
}
textarea:focus {
  outline: 2px solid #03bcdd;
}
textarea::placeholder {
  padding: 1rem;
  font-size: larger;
  color: #757272;
  margin: auto;
}
</style>