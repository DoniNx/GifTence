<template>
  <div>This is sample text</div>
  <input type="text" name="textBox" v-model="theSentence" id="inputBox">
  
  <button v-on:click="startOp()">
    Hello
  </button>

  <button v-on:click="startOp()">
    Arrange
  </button>

  {{ POS }}
  {{ theSentence }}
  {{ verifiedWords }}
  <br>
  <img id="box" v-for="gif in gifs" :src="gif" :key="gif">

  
</template>

<script>
export default {
  name: "SearchTab",
  data(){
    return{
      theSentence: '',
      wordIntel: [],
      verifiedWords: [],
      POS: [],
      gifs:[],
      ValidPartsOfSpeechs: [
        'verb',
        'adjective',
        'pronoun'
      ]
      }
  },
  methods: {
    startOp(){
      this.POS = [];
      this.gifs = [];
      this.verifiedWords = [];
      var arr = this.theSentence.split(/\s+/);
      for(let i of arr){
        this.fetchApi(i)
      }
    },
    async fetchApi(theWord) {
      
      let url = `https://api.dictionaryapi.dev/api/v2/entries/en/${theWord}`;


      await fetch(url)
        .then((response) => response.json())
        .then((result) => (this.wordIntel = result))
        .catch((err) => {
          console.log("can't find meaning");
        });

        
        this.filterPOS(theWord);
    },
    filterPOS(theWord){
       for(let i of this.wordIntel){
         console.log('THe wordIntel', this.wordIntel)
         i.meanings.forEach((ele,inx, arr)=>{
         
            if(this.ValidPartsOfSpeechs.includes(ele.partOfSpeech) && !this.verifiedWords.includes(theWord)){
              this.POS.push(ele.partOfSpeech)
              this.verifiedWords.push(theWord)
            }
         })
       }
       this.getGifs();
       console.log( 'verified words',this.verifiedWords);
    },
    async getGifs(){
      
      for (let i of this.verifiedWords) {
        await fetch(`https://api.giphy.com/v1/stickers/search?api_key=IVskIQMMlcVjKSRoxiggCvAnteKJqmXq&limit=1&q=${i}&tag=${i}&rating=pg&weirdness=0`)
        .then((response) => response.json())
        .then((result) => {
          let theUrl = result.data[0].images.original.url;
          if (!this.gifs.includes(theUrl)) {
            this.gifs.push(theUrl)
          }
          
          })
        .catch((err) => {
          console.log("can't find gif");
        })
      }
      
          console.log('gifs', this.gifs)

    },

    

  
  }

}
</script>



<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
#box{
  background-size: cover;
  border: #42b983 solid 4px;
  height: 200px;
  width: auto;

}
</style>




















