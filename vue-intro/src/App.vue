<template>
  <h1>How fancy are your fingers?</h1>
  <div>
    <span
    :key="word.position"
     v-for="word in fetchedText">
      {{ word.text }} {{ '' }}
    </span>
  </div>
  <textarea name="" id="" cols="30" rows="10"  :value="inputValue"></textarea>
  <button @click = fetchData()>Click</button>
</template>

<script>
export default {
  data() {
    return {
      inputValue: "",
      index: 0,
      fetchedText: [],
    };
  },
  methods: {
    async fetchData() {
      const response = await fetch("text/find_you.txt");
      const data = await response.text();
      let obj = data.trim().split(" ").map((word, index) => { 
        return {

            position: index,
            text: word,
            correct: false,
            wrong: false,
            pending: true
        }
     })
      return this.fetchedText = obj;
    },
  },
};
</script>