<template>
  <p>How fancy are your fingers?</p>
  <div class="data">
    <span :key="txt.text" v-for="txt in fetchedText">{{ txt.text }} </span>
  </div>
  <button @click="fetchData">Start test</button>
  <textarea
    name="typingArea"
    cols="30"
    rows="10"
    v-model= "inputValue"
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
          data.split('').forEach(l =>{
            let ob = {};
            l === '\n' ? '<br/>' : l;
            ob['text'] = l;
            ob['correct'] = false;
            ob['wrong'] = false;
            ob['pending'] = true;
            this.fetchedText.push(ob)
          })
        });
    },
    processInput(event){
          const value = event.target.value.length;

          if (value === "") {
              return;
          }
          if (this.fetchedText[this.index].text === value.trim().charAt(this.index)) {
            
              //correct answer
              this.fetchedText[this.index].correct = true
              this.fetchedText[this.index].wrong = false
              this.fetchedText[this.index].pending = false

              console.log(this.fetchedText[this.index])
          }

          this.index++
      }
  },
};
</script>
<style scoped>
</style>