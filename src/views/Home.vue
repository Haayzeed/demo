<template>
  <div class="home">
    <button @click.prevent="getShape" content="ellipse">Ellipse</button>
    <button @click.prevent="getShape" content="round">Round</button>
    <button @click.prevent="getShape" content="triangle">Triangle</button>
    <button @click.prevent="getShape" content="square">Square</button>
    <button @click.prevent="getShape" content="rectangle">Rectangle</button>

    <br>
    <button @click.prevent="getColor" content="red">Red</button>
    <button @click.prevent="getColor" content="green">Green</button>
    <button @click.prevent="getColor" content="blue">blue</button>

    <div class="content">
    <div v-for="(item) in filterItems | filterItemss" :key="item.id" :id="item.id" class="shape">
        <div > {{item.shape}}</div>
        <div> <img :src="'@/assets/' + item.image"> {{item.color}} </div>
    </div>
    </div>
    {{filterContent}}
  </div>
</template>

<script>
import axios from 'axios'
import items from '@/assets/data.json'
export default {
  name: 'Home',
  components: {

  },
  data () {
    return {
      items: items,
      shape: '',
      color: '',
      query: [],
      colorq: [],
      content: [],
      url: process.env.VUE_APP_URL
    }
  },
  computed: {
    filterItems: function(){
      return this.filterByShape(this.items)
    },
    filterItemss: function(){
      return this.filterByColor(this.items)
    },
    filterContent: function() {
      var contents = this.content
      var newData = []
       for(let i=0; i < contents.length; i++){
        newData.push(contents[i].title)
      }
      return newData;
    }

  },
  methods: {
    getShape(e) {
      e.target.classList.toggle('shape')
      let shapes = e.target.getAttribute('content')
      let index = this.query.indexOf(shapes)
      if (index >= 0) {
        this.query.splice(index, 1);
      }
      else {
        this.query.push(shapes)
        console.log(this.query)
      }
    },  
    getColor(e) {
      e.target.classList.toggle('color')
      let color = e.target.getAttribute('content')
      let index = this.colorq.indexOf(color)
      if (index >= 0) {
        this.colorq.splice(index, 1);
      }
      else {
        this.colorq.push(color)
        console.log(this.color)
      }
    },
    filterByShape: function(items){
      return items.filter(item => !item.shape.indexOf(this.query))
    },

    filterByColor: function(items) {
      return items.filter(item => !item.color.indexOf(this.colorq))
    },

    getContent() {
      
      axios.get(process.env.VUE_APP_URL).then(response => {
        this.content = response.data
        console.log(this.content)
      })
    }
  },
  mounted() {
    this.getContent()
    console.log(process.env.VUE_APP_URL);
    console.log(process.env.VUE_APP_KEY);
  }
}
</script>
<style lang="scss" scoped>
.home {
  button {
    &.shape {
      background: black;
      color: white;
    }
    &.color {
      border: 2px solid black;
    }
  }
}
  .content {
    padding: 50px;
    display: grid;
    grid-gap: 20px;
    grid-template-columns: repeat(3, 1fr);
    
  }
</style>
