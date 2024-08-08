<template>  
  <div class="container row "> 
      <span class="blend-difference">Key2Pay</span> 
      <img class="key" src="@/icons/Sin_titulo-3.ico"  style="z-index: 200;"/>  
      <h1 class=" blend-difference tittle" id="main-tittle" style="background-color: transparent;" >Key2Pay</h1>  
      <ul  class="blend-difference text-white nav" style=" z-index:200;" >  
        <li v-for="link in links" :key="link.id" class="mr-2">  
          <a href="" class="link-click">  
            <span  >{{ link.text }}</span>  
          </a>  
        </li>  
      </ul> 
      <v-btn class="login-btn text-none text-white " >Login</v-btn>
      
  </div>  
</template>  
<script setup>
import { gsap } from "gsap";
import { onMounted, ref } from "vue";
import SplitType from 'split-type'





let ctx;

const links = ref([
  { id: 1, text: "About Us" },
  { id: 2, text: "Help Center" },
  { id: 3, text: "Contact" },
  { id: 4, text: "Resources" },
  { id: 5, text: "Developers" },
]);

const isLast = (id) => {
  return id === links.value[links.value.length - 1].id;
};
onMounted(() => {

  const mainText = new SplitType('#main-tittle')
  
 

  gsap.to(".nav", {
     y:0,
     stagger:0.01,
     opacity:1,
     ease: "power3.out",     duration: 0.3,
     delay: 1.5,
  })
  
  gsap.to(".char", {
     y:0,
     stagger:0.04,
     ease: "power3.out",
     duration:0.000001,
   
     opacity:1,
     delay: 1.70,

  })

  gsap.to(".login-btn", {
     y:0,
     stagger:0.01,
     opacity:1,
     ease: "power3.out",     duration:0.3,
     delay: 1.5,
  })


  ctx = gsap.timeline();

  

  
  

  ctx.to(".key", {
    scale: 0.43,
    ease: "power3.out",
    duration:1,
  });



  ctx.to(".key", {
    y: -5,
    ease: "power3.out",
  });

  ctx.to(".key", {
    x: 138,
    opacity:1,
    delay:0.2,
    duration:1,
    ease: "power3.out",
  });
});



</script>

<style>
:root {  
  --animation-primary: 0.735s cubic-bezier(0.62, 0.05, 0.01, 0.99);  
  --main-grid-gap: var(--gap);
  --gap: calc(var(--title-size) * 0.2);
  --main-grid-gap: var(--gap);

}  




.container {
  width: 100%;
  position: relative;
  top: 0;
  left: 0;
  padding-left: 10px;
  padding-right: 10px;
  padding-top: 10px;
  display: flex;
  background-color: transparent;
  height: 70px;
}


.row{
    display: flex;
    width: 100%;
    justify-content: space-around;
    align-items: center;
}

.colums {
    flex-wrap: wrap;
    gap: var(--main-grid-gap) var(--main-grid-gap);
}

.char {  
  transform: translateY(100px);  
  transition: transform .5s;  
  opacity: 0;  
}  

.nav {  
  transform: translateY(-115px);  
  font-family: 'Helvetica Neue', sans-serif;
  transition: transform .5s;  
  opacity: 0;  
  position: fixed;   
  display: flex;
  text-decoration: none;
  
  list-style-type: none; /* Añadido para eliminar los puntos de la lista */  

}  

.key {  
  position: fixed;  
  left: -200px; 
  z-index: -1;  
  opacity: 0;  
}  


.login-btn { 
   position: fixed;
   font-family: Stratos, sans-serif !important;  
   font-size: 1.3rem;
   right: 40px;
   transform: translateY(-115px);  
   transition: transform .5s;  
   z-index: 200;
   background-color: #129cf6;
}

.tittle {  
  position: fixed;  
  left: 100px;  
  padding-bottom: 12px;  
  font-family: Stratos, sans-serif !important;  
  font-weight: 1em !important;  
  font-size: 40px;  
  letter-spacing: -0.06em;  
  color: rgb(248, 248, 248);

  z-index: 200;
  
}  

.v-toolbar__content {  
  height: 100px !important;  
}  

.link-click {  
  padding: 0.2em 3px;  
  display: inline-block;  
  position: relative;  
  cursor: pointer;  
  text-decoration: none;  
  color: white;   
  font-size: 1.1em;   
}  

.link-click::before {  
  content: "";  
  position: absolute;  
  bottom: -2px;  
  left: 0;  
  width: 100%;  
  height: 2px;  
  background-color: #129cf6;  
  transform-origin: right;  
  transform: scaleX(0) rotate(0.001deg);  
  transition: transform var(--animation-primary);  
}  

.link-click:hover::before {  
  transform-origin: left;  
  transform: scaleX(1);  
}  

.ul {  
  display: flex;  
  flex-direction: row;  
  list-style-type: none;  
  padding-inline-start: 0;   
}  

li {  
  text-decoration: none;   
  gap: 1px;  
  line-height: 1.1;  
}  

/* Clase para el modo de mezcla */  
.blend-difference {  
  mix-blend-mode: difference!important;  
}  

</style>

