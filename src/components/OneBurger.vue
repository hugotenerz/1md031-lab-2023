<template>
<div>
  <h3>{{burger.name}}</h3>
  <img :src="burger.url" :alt="burger.name" :width="burger.width" :height="burger.height">
      <section class="contents">
        <li>{{burger.contents1}}</li>
        <li> Contains: <span id="allergens">{{burger.all}}</span></li>
      </section>
      <button @click="add">Add</button>
      <button @click="reduce">Reduce</button>
      <p>Amount Ordered: {{amountOrdered}}</p>
    </div>
</template>

  <script>
  export default {
    name: 'OneBurger',
    props: {
      burger: Object
    },
  
  data: function () {
    return {
      amountOrdered: 0,
    }
  },

  methods: {
    add() {
      this.amountOrdered++;
      this.$emit('orderedBurger', { name:   this.burger.name, 
                                    amount: this.amountOrdered 
                                  }
      );
    },
    
      reduce() {
        this.amountOrdered--;
        this.$emit('orderedBurger', { name:   this.burger.name, 
                                      amount: this.amountOrdered 
                                    },
        );
      }
    }
  }
  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>
img{
  width: 200px;
}
  </style>
  