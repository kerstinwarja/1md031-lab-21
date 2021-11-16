
<template>
  <section>
    <header id= "head">
      <img src="https://www.appmachine.com/wp-content/uploads/2016/02/No-Branding-Header-Background-1.jpg">
      <h1> Välkommen till BurgerQueen</h1>
    </header>
  </section>
      
  <main>
    <section class="menuwrapper">
      <div>
        <h2> Välj Burgare </h2>
        Här väljer du din burgare
      </div>
      <div id= "burgerwrapper">
        <Burger 
            v-for="burger in burgers"
            v-bind:burger="burger"
            v-bind:key="burger.name"
        />  
      </div>

      <!--<div id= "burgerwrapper">
        <div class="burger1" style="float:left;">
            <h3> Halloumiburgare </h3>
            <img src="https://assets.icanet.se/e_sharpen:80,q_auto,dpr_1.25,w_718,h_718,c_lfill/imagevaultfiles/id_161415/cf_259/halloumiburgare_med_srirachamajonnas_och_soltorkade_tomater.jpg" >      
            <ul>
              <li>Lakto ovo vegetarisk</li>
              <li>Serveras med krispigt bröd</li>
              <li>Innehåller <span class="allergies">mjölk</span> och <span class="allergies">gluten</span></li>
            </ul>
        </div>
        <div class="burger2" style="float:left;">
          <h3>Kycklingburgare</h3>
          <img src="https://cdn.kronfagel.se/app/uploads/2019/07/Kycklingburgare-med-Garlic-Mozzarella.jpg" >
          <ul>
            <li>Gjord på svensk kyckling</li>
            <li>Prisbelönad caesarsås</li>
            <li>Innehåller <span class="allergies">mjölk</span> och <span class="allergies">gluten</span></li>
          </ul>
        </div>
        <div  class="burger3" style="float:left;">
          <h3>Smashed burger</h3>
          <img src="https://bbqlovers.se/wp-content/uploads/2017/04/Smashburgare6-282x300.jpg">
          <ul>
            <li>100% nötkött från Norrland</li>
            <li>Västerbottensost</li>
            <li>Innehåller <span class="allergies">mjölk</span> och <span class="allergies">gluten</span></li>
          </ul>
        </div>
      </div>-->
    </section>
            
    <section style="clear:left" id="customerinformation">
      <h2>Kundinformation</h2>
      Här lämnar du nödvändig information
      <h3>Leveransinformation</h3>
      <p>
        <label for="full name">Namn</label><br>
        <input type="text" id="full name" name="fn" required="required" placeholder="För- och efternamn">
      </p>
                
      <p>
        <label for="mejladress">E-mail</label><br>
        <input type="email" id="email" name="em" required="required" placeholder="E-mail adress">
      </p>
      <p>
        <label for="street">Gata</label><br>
        <input type="text" id="street" name="st" required="required" placeholder="Gatuadress">
      </p>
      <p>
        <label for="housenumber">Hus</label><br>
        <input type="number" id="housenumber" name="hn" required="required" placeholder="Husnummer">
      </p>
      <p>
        <label for="payment">Betalningsalternativ</label><br>
        <select id="payment" name="pm">
            <option>Swish</option>
            <option selected="selected">Betalning via bank</option>
            <option>Faktura</option>
        </select>
      </p>
      <p>
        <label for="gender">Välj kön</label><br>
        <input type="radio" id="man" name="gender" value="man">
        <label for="man">man</label><br>
        <input type="radio" id="woman" name="gender" value="woman">
        <label for="woman">kvinna</label><br>
        <input type="radio" checked="checked" id="do not wish to provide" name="gender" value="do not wish to provide">
        <label for="do not wish to provide">vill ej ange</label>
      </p>
                 
    </section>

    <section>
      <button type="submit">
        <img src="https://www.emojimeaning.com/img/img-apple-160/1f354.png" style="width: 15px;height: 15px;">
        Skicka beställningen!
      </button>
    </section>
  </main>
  <hr>
  <footer>
    © 2021 Kerstin AB 
  </footer>
  
  <div id="map" v-on:click="addOrder">
    click here
  </div>

</template>

<script>

import Burger from '../components/Burger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io();
//use this to create menuitems
/*function MenuItem(name,url,g,l,i1,i2){
  this.name=name;
  this.picture=url;
  this.gluten=g;
  this.lactose=l;
  this.info1=i1;
  this.info2=i2;
}*/
//array with my burgers
/*const burgerArray=[new MenuItem("Halloumiburgare","https://assets.icanet.se/e_sharpen:80,q_auto,dpr_1.25,w_718,h_718,c_lfill/imagevaultfiles/id_161415/cf_259/halloumiburgare_med_srirachamajonnas_och_soltorkade_tomater.jpg",true,true,"Lakto ovo vegetarisk","Serveras med krispigt bröd"),
                   new MenuItem("Kycklingburgare","https://cdn.kronfagel.se/app/uploads/2019/07/Kycklingburgare-med-Garlic-Mozzarella.jpg",false,true,"Gjord på svensk kyckling","Prisbelönad caesarsås"),
                   new MenuItem("Smashed burger","https://bbqlovers.se/wp-content/uploads/2017/04/Smashburgare6-282x300.jpg",true,true,"100% nötkött från Norrland","Västerbottensost")];
*/
export default {
  name: 'Home',
  components: {
    Burger
  },
  data: function () {
    return {
      //to use menuItem uncomment
      //burgers: burgerArray
      burgers:menu
              
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    addOrder: function (event) {
      let offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: event.clientX - 10 - offset.x,
                                           y: event.clientY - 10 - offset.y },
                                orderItems: ["Beans", "Curry"]
                              }
                 );
    }
  }
}
</script>

<style>
  #map {
    width: 300px;
    height: 300px;
    background-color: red;
  }
  @import 'https://fonts.googleapis.com/css?family=Pacifico|Dosis';
body {
    font-family:arial;
}
 
.allergies {  /*class*/
    font-weight: bold;  /* class="allergies" in bold font*/
 }
/*ALL SECTIONS*/
section {
   margin: 2px 10px 0px 10px;
   /*Margins top right bottom left*/
}
/*BUTTON*/
button:hover {
   background-color: #36a33a; /* Green */
   color: white;
   cursor: pointer;
}
button{
   margin: 10px 0px 30px 0px;
}
/*HEADER*/
#head{
   height: 150px;
   width: 100%;
   overflow: hidden;  
}

#head img{
   opacity:0.8;
}

#head h1{
   position: absolute; 
   margin-left: 33%;
   margin-top: -570px; 
}

/* MAIN  */
/*       */

/*Hela menyn*/
.menuwrapper{ 
   background-color: black;
   color: white;
   border: 3px dotted white;
}
.menuwrapper div{
   padding: 10px 40px 10px;
}
/*bara burgarna*/
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
.burger1{
   border: 1px dotted #ff9900; 
}
.burger2{
   border: 1px dotted #ff9900;  
}
.burger3{
   border: 1px dotted #ff9900;
}

/*CUSTOMERINFORMATION*/
#customerinformation {
   border: black dotted;
   background-color: white;
   color: black;
   padding-left: 20px;
}
</style>
