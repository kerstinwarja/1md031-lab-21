<template>

  <div class="burgerItem" style="float:left;">
    <h3> {{burger.name}} </h3>
    <img v-bind:src = burger.picture>   
    <ul>
      <li>{{ burger.info1 }}</li>
      <li>{{ burger.info2 }}</li>
      <li v-if="burger.lactose"> 
        Innehåller <span class="allergies">laktos</span> 
      </li>
      <li v-else><span class="allergies">Laktosfri</span></li>
      <li v-if="burger.gluten"> 
        Innehåller <span class="allergies"> gluten </span>
      </li>
      <li v-else><span class="allergies">Glutenfri</span></li>
    </ul>
    
    
    <div class="burgerbuttons">
      <button v-on:click="removeBurger" type="button" class="removebutton" style="float:left;">
       -
      </button>
      Antal: {{amountOrdered}}
      <button v-on:click="addBurger" type="button" class="addbutton">
        +
      </button>
    </div>

  </div>
</template>

<script>
export default {
  name: 'Burger',
  props: {
    burger: Object
  },
  data: function () {
    return {
      amountOrdered: null
    }
  },
  methods: {
    removeBurger: function (){
      this.amountOrdered -= 1;
      if(this.amountOrdered < 1 ){
        this.amountOrdered=null;
      }
      
    this.$emit('orderedBurger', { name:   this.burger.name,
                                amount: this.amountOrdered
                              }
    );
    
      
    },
    addBurger: function(){
      this.amountOrdered += 1;
      this.$emit('orderedBurger', { name:   this.burger.name,
                                amount: this.amountOrdered
                              }
  );

    }


  } 
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#burgerwrapper {
   display: grid;
   grid-gap: 10px;
   grid-template-columns: 33% 33% 33%;
}

#burgerwrapper img{
   padding-left: 40px;
   width: 200px;
   height: 200px;
}
#burgerwrapper h3{
   padding-left: 60px;
}
.burgerItem{
   border: 1px dotted #ff9900; 
}
.allergies {  /*class*/
    font-weight: bold;  /* class="allergies" in bold font*/
    color: salmon;
 }
section {
   margin: 2px 10px 0px 10px;
   /*Margins top right bottom left*/
}
.burgerbuttons{
  padding-left: 70px;
  
}
.addbutton{
  background:green; 
}
.removebutton{
  background:red
}




</style>
