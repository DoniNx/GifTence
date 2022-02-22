<template>
<div class="container">
<div class="gradBack">
  <div class="pt-10">
    <h1  class="text-3xl mb-1">GifTence</h1> 
    <h1  class="text-xl mb-6">Write <span class="text-red-800">Gif</span> Sen<span class="text-red-800">tence</span>s</h1>
    <input type="text" name="textBox" v-model="theSentence" id="inputBox" class="drop-shadow-md shadow-inner rounded-xl w-1/3 h-8 pl-2 focus:outline-none focus:border-slate-900 pt-1 border-slate-700 border-2 text-lg">
  </div>
  
  <div class="flex-row my-3">

  <button v-on:click="startOp()" class="mx-2 w-28 bg-slate-600 hover:bg-slate-700 px-3 py-1 text-gray-100 font-bold rounded-full">
    Go
  </button>

  <button v-on:click="startOp()" class="mx-2 w-28 bg-slate-500 hover:bg-slate-600 px-3 py-1 text-gray-100 font-bold rounded-full">
    Arrange
  </button>

  </div>
  </div>



  <!-- {{ POS }}
  {{ theSentence }}
  {{ verifiedWords }} -->
  <br>
  <div class="flex flex-row m-auto mt-2 border-orange-700 border-2 w-fit" :v-if="gifs.length">
  <img id="box" class="place-self-center" v-for="gif in gifs" :src="gif" :key="gif">
  </div>
</div>
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
        'pronoun',
        'noun'
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
  
  height: 200px;
  width: auto;

}
.gradBack{
  
  background: #8e9eab;  /* fallback for old browsers */
background: -webkit-linear-gradient(to right, #eef2f3, #8e9eab);  /* Chrome 10-25, Safari 5.1-6 */
background: linear-gradient(to right, #eef2f3, #8e9eab); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
border-bottom: solid slategrey 2px;
  width: 100%;
  padding-top: 5%;
  padding-bottom: 2%;

}


.container{
  display: flex;
  flex-direction: column;
  min-width: 100%;
}
</style>




















