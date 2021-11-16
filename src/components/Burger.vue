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
      <li v-if="burger.gluten"> 
        Innehåller <span class="allergies"> gluten </span>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'Burger',
  props: {
    burger: Object
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
 }
 section {
   margin: 2px 10px 0px 10px;
   /*Margins top right bottom left*/
}


</style>
