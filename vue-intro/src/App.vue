<template>
<p>How fancy are your fingers? </p>
<div class="data"><span 
:key="txt.text" v-for="txt in fetchedText"
>{{ txt }}
</span></div>
<button @click="fetchData">Start test</button>
<textarea name="typingArea" cols="30" rows="10"></textarea>
</template>

<script>
export default {
  data(){
    return{
      inputValue: '',
      fetchedText: [],
    }
  },
  methods: {
    fetchData(){
      fetch('text/free.txt')
      .then(res => res.text())
      .then(data => {
        data.split(' ');
        this.fetchedText = data
      })
    }
},

};
</script>
<style scoped>

</style>