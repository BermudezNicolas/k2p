<script setup>
import NavBar from "@/components/navBar.vue";
import Map from "@/components/map.vue"
import { gsap } from "gsap";
import { onMounted } from "vue";
import {ScrollTrigger} from "gsap/ScrollTrigger"
import SplitType from 'split-type'
import Lenis from 'lenis'





let ctx;
let elevate;
let elevate2;
let slide;
let slideSection;
onMounted(() => {
  

  gsap.registerPlugin(ScrollTrigger);  

  /* Panels */
  const cont = document.querySelector("#panels-container");
  const panels = gsap.utils.toArray("#panels-container .panel");

 document.addEventListener("DOMContentLoaded", function () {
     const cards = [
       {id: "#spei",rotate: 43},
       {id: "#westerUnion", rotate: -13},
       {id: "#sepa", endTransalateX: 5500, rotate: -10},
       {id: "#nequi", rotate: -30},
       {id: "#swift", endTransalateX: 3500, rotate: -10},
       {id: "#codi", endTransalateX: 3500, rotate: 10},
       {id: "#pix", rotate: -50},
       {id: "#walmart", endTransalateX: 3500, rotate: -20},
       {id: "#khipu", endTransalateX: 3500, rotate: -10},
       {id: "#upi", endTransalateX: 3500, rotate: 20},
       {id: "#etPay", endTransalateX: 3500, rotate: 190},
       {id: "#klap",  rotate: 10},


     ];


     cards.forEach((card) => {  
    ScrollTrigger.create({  
      trigger: card.id,  
      start: "top top",  
      end: "+=1400",  
      markers: true,  
      scrub: 1,  
      onUpdate: (self) => {  
        gsap.to(card.id, {  
          x: `${card.endTranslateX * self.progress}px`,  
          rotate: `${card.rotate * self.progress * 2}`,  
          duration: 0.5,  
          ease: "power3.out",  
        });  
      }  
    });  
  
    
  });  
  })

 

  elevate = gsap.timeline( );
  elevate.to("section-2", {
    y:-100,
    ease: "power2.out",
    scrollTrigger:{
      trigger: ".section-1",
      start:"-70px top",  
      scrub: 6,  
      pinSpacing: false, 
      end: () => "+=" + (cont.scrollWidth - innerWidth),
      pin:true,
      anticipatePin:1,
    }   
   
  });



  
  const lenis = new Lenis()

  lenis.on('scroll', (e) => {
    console.log(e)
  })

  lenis.on('scroll', ScrollTrigger.update)

  gsap.ticker.add((time)=>{
    lenis.raf(time * 700)
  })

  
  const splitTypes = document.querySelectorAll('.reveal-type');
  splitTypes.forEach((char,i) => {
    const text = new SplitType(char, {types : 'chars'})
    elevate.fromTo(text.chars, 
      {
        opacity: 0
      },
      {
        scrollTrigger: {
        
        trigger: ".section-2",
        start: 'top top',
        end: () => "+=3000",
        pin:true,
        scrub:true,
        markers: true,
        },
        opacity: 1,
        stagger: 0.1,
        anticipatePin:true,
        
      }

    )
  })

  
  slide = gsap.timeline();  

  // Para #info-1: deslizamiento justo al entrar en la sección  
  slide.to("#info-1", {  
      x: 1480, 
      ease: "power2.out",
      scrollTrigger: {  
          trigger: ".section-3",  
          pin:true,
          start: "top top", // Comienza en la parte superior de la sección 3  
          end: () => "+=" + (cont.scrollWidth - innerWidth),
          scrub: 3,  
       },
        
  })
  
  
  // Para #info-2: deslizar más tarde  
  slide.to("#info-2", {  
      x: -1280,  
      ease: "power2.out",
      scrollTrigger: {  
          trigger: ".section-3",  
          start: "top top", // Comienza 200px desde la parte superior de la sección 3  
          pin:"section-3",
          end: () => "+=" + (cont.scrollWidth - innerWidth),
          scrub: 3,  
          anticipatePin: true, 

  
          // ya no es necesario establecer `pin: ".section-3"` aquí si ya está definido para info-1  
      }, 
  });


  slide.to("#info-3", {  
      x: 1480,  
      ease: "power2.out",
      scrollTrigger: {  
          trigger: ".section-3",  
          start: "top top", // Comienza en la parte superior de la sección 3  
          end: () => "+=" + (cont.scrollWidth - innerWidth),
          scrub: 3,  
          anticipatePin: false, 
           
      },
     
        
  })
  
  slide.to("#info-4", {  
      x: -1280, 
      ease: "power2.out", 
      scrollTrigger: {  
          trigger: ".section-3",  
          start: "top top", // Comienza en la parte superior de la sección 3  
          end: () => "+=" + (cont.scrollWidth - innerWidth),
          scrub: 3,  
          anticipatePin: false, 
          
           
      },
     
        
  })






  slideSection = gsap.timeline();

  slideSection.to(panels, {
    x: () => -1 * (cont.scrollWidth - innerWidth),
    ease: "power2.out",
    scrollTrigger: {
      trigger: "#panels",
      pin: true,
      start: "top top",
      scrub: 1,
      end: () => "+=" + (cont.scrollWidth - innerWidth),
      onUpdate: (self) => {
      console.log(self.progress)
      }
    }
  });

  document.addEventListener("DOMContentLoaded", () => {  
  const logo = document.querySelector("#key");   
  
  let logoToScale = () => {  
    if (logo.offsetWidth) {  
      return (window.innerWidth / logo.offsetWidth) * 170; // Cambiado a 0.5 para que no se agrande tanto  
    }  
    return 1; // Valor por defecto si no se obtiene el ancho  
  };  

  const timelineHeader = gsap.timeline({  
    scrollTrigger: {  
      id: "ZOOM",  
      trigger: "div.landing",  
      scrub: 1,  
      start: "top top",  
      end: "+=100%",  // Cambiado para que el final sea más claro  
      pin: "#landing",  
    
      invalidateOnRefresh: true   
    }  
  });  

  timelineHeader.to(logo, {  
    
    ease: "power2.inOut",  
    scale: logoToScale // Asegúrate de que sea un número válido  
  });  

  const projects = document.querySelector("section.projects");  

  timelineHeader.to(projects, {  
    ease: "power2.inOut",  
    x: () => -(projects.scrollWidth - window.innerWidth),  
    scrollTrigger: {  
      trigger: projects,  
      pin: "#landing-2",  
      start: 'center top',  
      end: () => `+=${projects.scrollWidth - window.innerWidth}`,  
      scrub: 1,  
      invalidateOnRefresh: true,  
     
    }  
  });  

  // Agregar un segundo to para escalar el logo hacia abajo o ocultarlo  
  timelineHeader.to(logo, {  
    duration: 1.5,  
    ease: "power2.inOut",  
    scale: 0, // Escalar a 0 para ocultar el logo al final  
    scrollTrigger: {  
      trigger: projects,  
      start: "top top", // Cambiado a "top center" para que inicie antes  
      end: "bottom center", // Cambiado para que termine cuando la parte inferior de projects sale del viewport  
      scrub: 1,  
      markers: true, // Mantener para depuración  
    }  
  });  
});  

  gsap.ticker.lagSmoothing(0)

  ctx = gsap.timeline();

  ctx.to("#create1", {
    ease: "power4.out",

    y: "0%",
    opacity: 1,
    stagger: 4,
    duration: 1.5,
    delay: 0.003,
  }),
  
  gsap.to("#create2", {
      ease: "power3.out",
      y:"0%",
      opacity: 1,
      stagger: 4,
      duration: 1.5,
      delay: 0.004,
    });

  gsap.to("#create3", {
    ease: "power3.out",
    y: "0%",
    opacity: 1,
    stagger: 4,
    duration: 1.5,
    delay: 0.005,
  });

  gsap.to("#background-img", {
    ease: "power3.out",
    opacity: 0.06,
    duration: 1,
    delay: 0.8,
  });

  gsap.to("#img1-k2p", {
    ease: "power3.out",
    x:390,
    opacity: 0.9,

    duration: 0.8,
    delay: 1.8,
  });


  gsap.to("#paymentSolutions", {
    y:-40,
    opacity:1,
    duration:0.5,
    delay:1.9,

  })


  gsap.to("#photo1", {
    x:-400,
    opacity:1,
    duration:1,
    scrub:true,
    scrollTrigger: {
      trigger:"#photo1",
      start:"top center",
    }
  })



  
});

</script>

<template>
  <main>
    
    <NavBar />
    <section id="section-1" class="section-1" >
          <div
          id="background-img"
          style="
            background-image: url(https://key2pay.online/wp-content/uploads/2024/03/k2pRecurso-1llave.png);
            height: 700px;
            width: 100%;
            background-repeat: no-repeat;
            background-size: cover;
            opacity: 0;
            position:relative;
            top:0;

          "
        ></div>
        <div>
          <img id="img1-k2p" decoding="async" width="500" height="400" src="https://key2pay.online/wp-content/uploads/2024/04/k2p_businessRecurso-1graphics_portada.png" class="attachment-full size-full wp-image-1884" alt="Tech" srcset="https://key2pay.online/wp-content/uploads/2024/04/k2p_businessRecurso-1graphics_portada.png 3562w, https://key2pay.online/wp-content/uploads/2024/04/k2p_businessRecurso-1graphics_portada-300x224.png 300w, https://key2pay.online/wp-content/uploads/2024/04/k2p_businessRecurso-1graphics_portada-1024x765.png 1024w, https://key2pay.online/wp-content/uploads/2024/04/k2p_businessRecurso-1graphics_portada-768x574.png 768w, https://key2pay.online/wp-content/uploads/2024/04/k2p_businessRecurso-1graphics_portada-1536x1148.png 1536w, https://key2pay.online/wp-content/uploads/2024/04/k2p_businessRecurso-1graphics_portada-2048x1531.png 2048w" style="position: absolute; bottom:240px; left:-300px; opacity:0;"></img>
        </div>  
        <div class="make-payments">
          <div class="text-start" style="margin-right: 130px; z-index: 100">
            <h6>
              <span
                id="create1"
                class="text-white blend-difference"
                style="z-index: 100"
              >
                The best way
              </span>
            </h6>
            <h6>
              <span id="create2" class="text-white blend-difference">
                To make
              </span>
            </h6>
            <h6>
              <span id="create3" class="text-white blend-difference">
                Payments
              </span>
            </h6>

            <div id="paymentSolutions" class="d-flex" style="padding-top: 100px; width: auto; opacity:0">
              <div class="d-flex align-center text-white mr-10" style="margin-left: 0">
                <span class="paymentTips mr-2">
                  250+
                          
                </span>
                <span style="font-family: Stratos, sans-serif !important;   font-size: 1.2em; margin-right:1">Payment</span>
                <span style="font-family: Stratos, sans-serif !important;   font-size: 1.2em;">Solutions</span>
              </div>

              <div class="d-flex align-center text-white ">
                <span class="paymentTips mr-2">
                  100+
                          
                </span>
                <span style="font-family: Stratos, sans-serif !important;   font-size: 1.2em;">Countries</span>
              </div>
            </div>
          </div>
        </div>
    </section>
    <section id="section-2" class="section-2">
        <div> 
          <div>
            <h1 class="reveal-type text-start text-white" style=" padding:70px">
              Our mission is to provide a <span style="color: #1597f5;">payment service</span> <br> as efficient as possible, to improve the user <br> experience in any <span style="color: #1597f5;">transaction</span> that exists.
            </h1>
           
          </div>
        </div>
    </section>
     
   
    <section id="section-3" class="section-3 d-flex" style="padding-top: 110px;">
      <div id="info" class="my-auto">
        <div class="d-flex info-left" id="info-1">
          <div class="d-flex align-center justify-center" style="border-radius: 100%; background-color:#1597f5; height:80px; width:80px;"><v-icon icon="mdi-cogs" color="white" size="50px"></v-icon></div>
          <div class="info-tittle">
             Easy to Setup
             <div class="info-para">
              Protect your customers' sensitive information with advanced encryption technology, ensuring every transaction is secure and worry-free.
             </div>
          </div>
        </div>
        <div class="d-flex info-right" id="info-2" >
          <div class="d-flex align-center justify-center" style="border-radius: 100%; background-color:#1597f5; height:80px; width:80px;"><v-icon icon="mdi-lock" color="white" size="50px"></v-icon></div>
          <div class="info-tittle">
            Secure Transactions
             <div class="info-para">
              Protect your customers' sensitive information with advanced encryption technology, ensuring every transaction is secure and worry-free.
             </div>
          </div>
          
       </div>
       <div class="d-flex info-left" id="info-3">
        <div class="d-flex align-center justify-center" style="border-radius: 100%; background-color:#1597f5; height:80px; width:80px;"><v-icon icon="mdi-map-marker" color="white" size="50px"></v-icon></div>
        <div class="info-tittle">
          Locals Payins & Payouts
           <div class="info-para">
            Process your payments with settlement of international, local, and/or hybrid funds.
           </div>
        </div>
        
     </div>
     <div class="d-flex info-right" id="info-4">
        <div class="d-flex align-center justify-center" style="border-radius: 100%; background-color:#1597f5; height:80px; width:80px;"><v-icon icon="mdi-message-text" color="white" size="50px"></v-icon></div>
          <div class="info-tittle">
            Dedicated Merchant Support
            <div class="info-para">
              Rest easy knowing that our dedicated support team is available round-the-clock to assist you with any inquiries or issues you may encounter. 
            </div>
          </div>
        </div>
     </div>
     <div>
      <img id="photo1" src="https://key2pay.online/wp-content/uploads/2024/01/aurora_Recurso-1busi.png"  width="399" height="385" alt="" style="position:absolute; bottom:60px; z-index:-1">
    </div>
    </section>
    <section id="panels" class="section-4">

      <div id="panels-container" style="width: 290%;">
        <article id="panel-2" class="panel full-screen red" >
          <div class="container-1 d-flex align-cemter justify-center">
            <h1 style="font-size: 22vw;color:white;  font-weight:500; z-index:2"> <span style="margin-right: 150px;"> </span> A whole world of payments</h1>
            
          </div>
          <div class="card" id="spei" style="position: absolute; top:23%; left:33%;  z-index:2;">

            <img src="@/icons/spei.png" alt="">
          </div>
          <div class="card" id="westerUnion" style="position: absolute;top:16%;  left:66%; z-index:2;">

            <img src="@/icons/westerUnion.png" alt="">
          </div>
          <div class="card" id="sepa" style="position: absolute;  top:42%; left:72%; z-index:2; ">

            <img src="@/icons/sepa.png" alt="">
          </div>
          <div class="card" id="swift" style="position: absolute; top:18%; left:87%; z-index:2; ">

            <img src="@/icons/swift.png" alt="">
          </div>
          <div class="card" id="nequi" style="position: absolute; top:51%;left:49%;z-index:2; ">

            <img src="@/icons/nequi.png" alt="">
          </div>
          <div class="card" id="codi" style="position: absolute;  top:63%; left:66%; z-index:2;">

            <img src="@/icons/codi.png" alt="">
          </div>
          <div class="card" id="pix" style="position: absolute;  top:55%; left:34%; z-index:2;  ">

            <img src="@/icons/pix.png" alt="">
          </div>
          <div class="card" id="walmart" style="position: absolute; top:18%; left:42%;  z-index:2; ">

            <img src="@/icons/walmart.png" alt="">
          </div>
          <div class="card" id="khipu" style="position: absolute; top:22%; left:95%; z-index:2;">

            <img src="@/icons/khipu.png" alt="">
          </div>
          <div class="card" id="upi" style="position: absolute; top:10%; left:50%;  z-index:2;">

            <img src="@/icons/upi.png" alt="">
          </div>
          <div class="card" id="etPay" style="position: absolute; top:60%; left:93%;  z-index:2;">

            <img src="@/icons/etPay.png" alt="">
          </div>
          <div class="card" id="klap" style="position: absolute;top:56%; left:82%;  z-index:2 "> 

            <img src="@/icons/klap.png" alt="">
          </div>
        </article>
      
      
        
      </div>
     
    </section>
    <section  id="landing" style="background-color: #091321; height:100vh; " >
      <div class="landing text-center" >
        <h1 id="key" style="font-size: 22vw;color:white;  font-weight:500; z-index:2; padding-left:20px;">methods</h1>
      </div>

    </section>
    <div id="landing-2" class="sections" style="background-color: white; height:100vh; ">
      <section class="projects" style="height:100vh">
        <div class="text-center" style="padding-top: 60px; "> 
           <Map />
        </div>
      </section>
   </div>
   <section style="background-color: white; width:100%; height:100vh; z-index:200">
      <h1 style="z-index:200">efsefsefse</h1>
   </section>
       


 
   


  </main>
</template>

<style>
:root {
  --color-light-rgb: 255, 255, 255;
}



.make-payments {
  width: 520px;
  position: relative;
  background-color: transparent;
  height: auto;
  display: flex;
  justify-content: end;
  align-items: center;
  position: absolute;
  bottom:240px;
  left: 800px;
}

h6 {
  font-size: 66px;
  letter-spacing: -0.035em;
  padding: 26px 0.1em;
  line-height: 0.5;
  overflow: hidden;
}

h6 span {
  display: inline-block;
  transform: translateY(200px);
  opacity: 0;
  font-weight: 600 !important;
  overflow: visible;
}

.optimal-pay {
  font-family: "Helvetica Neue", sans-serif;
  font-size: 20px;
}

.logo {
  font-size: 300px;
  color: white;
  font-family: "Helvetica Neue", sans-serif;
  font-weight: 100;
}

.blend-difference {
  mix-blend-mode: difference !important;
}


.paymentTips {
  font-family: Stratos, sans-serif !important;  
  font-size: 2em;
}

.section-1 {
  width: 100%;
  height: 700px;
  z-index: 1;
  background-color: transparent;
  position: relative;
}

.section-2{
  background-color:#091321;
  width: 100%;
  height: 100vh;
  z-index: 1;
  margin: 0;
  padding: 0;
  position: relative;
}

.section-3{
  background-color: white;
  width: 100%;
  height: 100vh;
  
  z-index: 1;
  margin: 0;
  padding: 0;
  position: relative;
}


.section-4{

  width: 100%;
  height: 100vh;
  
  z-index: 1;
  margin: 0;
  padding: 0;
  position: relative;
}




#panels #panels-container {
  height: 100%;
  display: flex;
  flex-wrap: nowrap;
  padding: 0;
  overflow: hidden;
  background-color: #091321;
}


.container-1 {
  width: 500%;
  height: 100vh;
  background-color:#091321;
}


#panels #panels-container .panel {
  position: relative;
  min-width: 100vw;
  height: 100vh;
  overflow: hidden;
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  color: white;
  text-align: left;
}



.reveal-type { 
   font-size:50px; font-weight:400;
}


.info-left { 
  position: relative;
  right: 1300px;
}

.info-right {
  position: relative;
  left: 1300px;
}


.info-tittle {
  color: #00214F;
  font-size: 25px;
  margin-left: 20px;
  margin-bottom: 20px;
  font-weight: 600;

}

.info-para{
  color: black;
  font-size: 20px;
  width: 70%;
}



</style>