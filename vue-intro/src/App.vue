<template>
  <p>How fancy are your fingers?</p>
  <div class="data">
    <span :key="txt.text" v-for="txt in fetchedText">{{ txt }} </span>
  </div>
  <button @click="fetchData">Start test</button>
  <textarea
    name="typingArea"
    cols="30"
    rows="10"
    v-model="inputValue"
    @keyup="processInput($event)"
  ></textarea>
</template>

<script>
export default {
  data() {
    return {
      index: 0,
      inputValue: "",
      fetchedText: [],
    };
  },
  methods: {
    fetchData() {
      fetch("text/find_you.txt")
        .then((res) => res.text())
        .then((data) => {
          data
            .trim()
            .split()
            .map((key) => {
              return {
                text: key,
                correct: false,
                wrong: false,
                pending: true,
              };
            });
          this.fetchedText = data;
        });
    },
    processInput(event){
      const value = event.target.value
      let valueIndex = value.length + 1;

      if (value === "") {
        return;
      }
      if (this.fetchedText[this.index] === value.charAt(valueIndex - 1)) {
        //correct answer
        console.log(true)
              this.keywords[this.index].correct = true
              this.keywords[this.index].wrong = false
              this.keywords[this.index].pending = false
      }
      this.index++;
      valueIndex++;
    }
  },
};
</script>
<style scoped>
</style>