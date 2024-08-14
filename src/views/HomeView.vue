<script setup>
import NavBar from "@/components/navBar.vue";
import { gsap } from "gsap";
import { onMounted } from "vue";
import {ScrollTrigger} from "gsap/ScrollTrigger"
import SplitType from 'split-type'
import Lenis from 'lenis'





let ctx;
let elevate;
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
       {id: "#westerUnion", rotate: -48},
       {id: "#sepa", endTransalateX: 5500, rotate: -40},
       {id: "#nequi", endTransalateX: 5500, rotate: 30},
       {id: "#swift", endTransalateX: 3500, rotate: -20},
       {id: "#codi", endTransalateX: 3500, rotate: 30},
       {id: "#pix", endTransalateX: 3500, rotate: 6},
       {id: "#walmart", endTransalateX: 3500, rotate: -10},
       {id: "#khipu", endTransalateX: 3500, rotate: -17 },
       {id: "#upi", endTransalateX: 3500, rotate: 180},
       {id: "#etPay", endTransalateX: 3500, rotate: 190},
       {id: "#klap", endTransalateX: 3500, rotate: 7},


     ];


     cards.forEach((card) => {  
    ScrollTrigger.create({  
      trigger: card.id,  
      start: "top top",  
      end: "+=3000",  
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
   
  })
 
 

  
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
        end: () => "+=" + (cont.scrollWidth - innerWidth),
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
          markers:true,
          anticipatePin: false, 

  
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
              Our mission is to provide a <span style="color: #1597f5;">payment service</span> as efficient as possible, to improve the user experience in any <span style="color: #1597f5;">transaction</span> that exists.
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
    <section id="panels">

      <div id="panels-container" style="width: 388%;">
        <article id="panel-2" class="panel full-screen red" style="width: 500%;">
          <div class="container-1 d-flex align-cemter justify-center">
            <h1 style="font-size: 22vw;color:#091321  ;  font-weight:500; z-index:2">A whole world of payments <span id="methods">methods</span></h1>
            
          </div>
          <div class="card" id="spei" style="position: absolute; top:23%; left:2360px;  z-index:2;">

            <img src="@/icons/spei.png" alt="">
          </div>
          <div class="card" id="westerUnion" style="position: absolute;top:16%;  left:66%; z-index:2;">

            <img src="@/icons/westerUnion.png" alt="">
          </div>
          <div class="card" id="sepa" style="position: absolute;  top:42%; left:2574px; z-index:2; ">

            <img src="@/icons/sepa.png" alt="">
          </div>
          <div class="card" id="swift" style="position: absolute; top:18%; left:60%; z-index:2; ">

            <img src="@/icons/swift.png" alt="">
          </div>
          <div class="card" id="nequi" style="position: absolute; top:51%;left:58%;z-index:2; ">

            <img src="@/icons/nequi.png" alt="">
          </div>
          <div class="card" id="codi" style="position: absolute;  top:53%; left:64%; z-index:2;">

            <img src="@/icons/codi.png" alt="">
          </div>
          <div class="card" id="pix" style="position: absolute; top:58%; left:79%; ">

            <img src="@/icons/pix.png" alt="">
          </div>
          <div class="card" id="walmart" style="position: absolute; top:13%; left:83%;  z-index:2; ">

            <img src="@/icons/walmart.png" alt="">
          </div>
          <div class="card" id="khipu" style="position: absolute; top:25%; left:74%; z-index:2;">

            <img src="@/icons/khipu.png" alt="">
          </div>
          <div class="card" id="upi" style="position: absolute; top:58%; left:88%; ">

            <img src="@/icons/upi.png" alt="">
          </div>
          <div class="card" id="etPay" style="position: absolute; top:15%; left:93%;  z-index:2;">

            <img src="@/icons/etPay.png" alt="">
          </div>
          <div class="card" id="klap" style="position: absolute; top:48%; left:95%; z-index:2; ">

            <img src="@/icons/klap.png" alt="">
          </div>
        </article>

        
       
        
      </div>
    </section>
    <section>
       
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



#panels #panels-container {
  height: 100vh;
  display: flex;
  flex-wrap: nowrap;
  padding: 0;
  overflow: hidden;
  background-color: #ddd;
}


.container-1 {
  width: 500%;
  height: 100vh;
  background-color:white;
}


#panels #panels-container .panel {
  position: relative;
  min-width: 100vw;
  height: 100vh;
  overflow: hidden;
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  color: #333;
  text-align: left;
  border-right: 1px solid #f00;
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