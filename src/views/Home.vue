
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
            v-on:orderedBurger="addToOrder($event)"
        />  
      </div>

    </section>
            
    <section style="clear:left" id="customerinformation">
      <h2>Kundinformation</h2>
      Här lämnar du nödvändig information
      <h3>Leveransinformation</h3>
      <p>
        <label for="full name">Namn</label><br>
        <input type="text" id="full name" v-model="fn" required="required" placeholder="För- och efternamn">
      </p>
                
      <p>
        <label for="mejladress">E-mail</label><br>
        <input type="email" id="email" v-model="em" required="required" placeholder="E-mail adress">
      </p>
      <!--<p>
        <label for="street">Gata</label><br>
        <input type="text" id="street" v-model="st" required="required" placeholder="Gatuadress">
      </p>
      <p>
        <label for="housenumber">Hus</label><br>
        <input type="number" id="housenumber" v-model="hn" required="required" placeholder="Husnummer">
      </p>-->
      <p>
        <label for="payment">Betalningsalternativ</label><br>
        <select id="payment" v-model="pm">
            <option>Swish</option>
            <option selected="selected">Betalning via bank </option>
            <option>Faktura</option>
        </select>
      </p>
      <p>
        <label for="gender">Välj kön</label><br>
        <input type="radio" id="man"  v-model="gender" value="man">
        <label for="man">man</label><br>
        <input type="radio" id="woman"  v-model="gender" value="woman">
        <label for="woman">kvinna</label><br>
        <input type="radio"  id="do not wish to provide" v-model="gender" value="do not wish to provide">
        <label for="do not wish to provide"> vill ej ange</label>
        
      </p>
      Ange leveransadress på kartan:
      
      <div class="mapwrapper">
        <div id="map" v-on:click="setLocation">
            <div
              v-bind:style="{
                left: location.x + 'px',
                top: location.y + 'px',
              }" 
            >
              T
            </div>
          </div>
      </div>
                 
    </section>
    

    <section>
      <button v-on:click="sendInfoClick" type="submit">
        <img src="https://www.emojimeaning.com/img/img-apple-160/1f354.png" style="width: 15px;height: 15px;">
        Skicka beställningen!
      </button>
    </section>
  </main>
  <hr>
  <footer>
    © 2021 Kerstin AB 
  </footer>
  

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
/*const burgerArray=[new MenuItem("Halloumiburgare","https://assets.icanet.se/e_sharpen:80,q_auto,dpr_1.25,w_718,h_718,c_lfill/imagevaultfiles/id_161415/cf_259/halloumiburgare_med_srirachamajonnas_och_soltorkade_tomater.jpg",true,false,"Lakto ovo vegetarisk","Serveras med krispigt bröd"),
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
      //burgers: burgerArray
      burgers:menu,
      fn: "",
      em:"",
      //st:"",
      //hn:"",
      pm:"Swish",
      gender:"do not wish to provide",
      orderedBurgers:{},
      location:{x:0,y:0},
      personalInformation:""
              
    }
  },
  methods: {
    sendInfoClick: function () {
      this.personalInformation = {
        name: this.fn,
        email: this.em,
        payment: this.pm,
        gender: this.gender
      }
      console.log(this.personalInformation,this.orderedBurgers);

      socket.emit("addOrder", {
        orderId: this.getOrderNumber(),
        details: this.location,
        orderItems: this.orderedBurgers,
        personalInformation:this.personalInformation
        
      });
      alert("order skickad!")
      
    },
    
    addToOrder: function (event) {
      this.orderedBurgers[event.name] = event.amount;
      
  },
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    /*addOrder: function () {

      let offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: event.clientX - 10 - offset.x,
                                           y: event.clientY - 10 - offset.y },
                                orderItems: ["Beans", "Curry"]
                              }
                 );
    },*/
    
    setLocation: function (event) {
      var offset = {
        x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top,
      };
      this.location = {
          x: event.clientX - 10 - offset.x,
          y: event.clientY - 10 - offset.y,
      };
    }
  },
};
</script>

<style>
  .mapwrapper{
    height:500px;
    width:1000px;
    overflow:scroll;
    margin-left: 10px;
    margin-bottom: 20px;
  }
  #map {
    width: 1920px;
    height: 1078px;
    background:url("/img/polacks.jpg");
    cursor: crosshair;
    background-repeat: no-repeat;
    position: relative;

  }
  #map div{
  position: absolute;
  background: black;
  color: white;
  border-radius: 10px;
  width:20px;
  height:20px;
  text-align: center;
}
  @import 'https://fonts.googleapis.com/css?family=Pacifico|Dosis';
body {
    font-family:arial;
}
 
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

/*CUSTOMERINFORMATION*/
#customerinformation {
   border: black dotted;
   background-color: white;
   color: black;
   padding-left: 20px;
}
</style>
