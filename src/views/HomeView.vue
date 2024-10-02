<script setup>
import NavBar from "@/components/navBar.vue";
import Map from "@/components/map.vue"
import { gsap } from "gsap";
import { onMounted, ref, nextTick } from "vue";
import {ScrollTrigger} from "gsap/ScrollTrigger"
import SplitType from 'split-type'
import Lenis from 'lenis'






let ctx;
let ctx1;
let elevate;
let elevate2;
let slide;
let slideSection;

const cards = [{tittle:"Payments Methods", text:"Choose the payment method of your preference: cash, online or credt card."},
               {tittle:"Converged Commerce", text:"Use one system for online, in-app, POS or in-store payments."},
               {tittle:"Single API", text:"For frictionless onboarding, security, rich functionality and flexibility."},
               {tittle:"Global Transfers", text:"SEPA: Send euros quickly and easily within Europe. SWIFT: Transfer funds globally with confidence."},
               {tittle:"Simple Terms", text:"Clear pricing, fast settlements and decision making. 48 hour onboarding."},
               {tittle:"The Best Service ", text:"24/7/365 customer support service and a dedicated local account manager."},
]








onMounted( async () => {
  gsap.registerPlugin(ScrollTrigger);
    
  const lenis = new Lenis()

  lenis.on('scroll', (e) => {
    console.log(e)
  })

  lenis.on('scroll', ScrollTrigger.update)

  gsap.ticker.add((time)=>{
    lenis.raf(time * 700)
  })


  gsap.ticker.lagSmoothing(0)


  const initialClipPaths = [
  "polygon(0% 0%, 0% 0%, 0% 0%, 0% 0%)",
  "polygon(33% 0%, 33% 0%, 33% 0%, 33% 0%)",
  "polygon(66% 0%, 66% 0%, 66% 0%, 66% 0%)",
  "polygon(0% 33%, 0% 33%, 0% 33%, 0% 33%)",
  "polygon(33% 33%, 33% 33%, 33% 33%, 33% 33%)",
  "polygon(66% 33%, 66% 33%, 66% 33%, 66% 33%)",
  "polygon(0% 66%, 0% 66%, 0% 66%, 0% 66%)",
  "polygon(33% 66%, 33% 66%, 33% 66%, 33% 66%)",
  "polygon(66% 66%, 66% 66%, 66% 66%, 66% 66%)"
];

const finalClipPaths = [
  "polygon(0% 0%, 33.5% 0%, 33.5% 33%, 0% 33.5%)",
  "polygon(33% 0%, 66.5% 0%, 66.5% 33%, 33% 33.5%)",
  "polygon(66% 0%, 100% 0%, 100% 33%, 66% 33.5%)",
  "polygon(0% 33%, 33.5% 33%, 33.5% 66%, 0% 66.5%)",
  "polygon(33% 33%, 66.5% 33%, 66.5% 66%, 33% 66.5%)",
  "polygon(66% 33%, 100% 33%, 100% 66%, 66% 66.5%)",
  "polygon(0% 66%, 33.5% 66%, 33.5% 100%, 0% 100%)",
  "polygon(33% 66%, 66.5% 66%, 66.5% 100%, 33% 100%)",
  "polygon(66% 66%, 100% 66%, 100% 100%, 66% 100%)"
];

function createMasks() {
  const imgs = document.querySelectorAll(".img");
  imgs.forEach((img) => {
    for (let i = 0; i < 9; i++) {
      const mask = document.createElement("div");
      mask.classList.add("mask", `m-${i + 1}`);
      img.appendChild(mask);
    }
  });
}

async function setupAnimations() {
  await nextTick(() => {
    createMasks();

    const rows = document.querySelectorAll(".row");
    rows.forEach((row) => {
      const imgs = row.querySelectorAll(".img");
      imgs.forEach((img) => {
        const masks = img.querySelectorAll(".mask");
        
        // Inicializa las máscaras
        masks.forEach((mask, index) => {
          gsap.set(mask, {
            clipPath: initialClipPaths[index]
          });
        });

        const tl = gsap.timeline({
          scrollTrigger: {
            trigger: row,
            start: "top 65%", // Cambiado para que inicie cuando esté más cerca del viewport
          
            onEnter: () => console.log('Entrada en viewport'),
          }
        });

        const animationOrder = [
          [".m-1"],
          [".m-2", ".m-4"],
          [".m-3", ".m-5", ".m-7"],
          [".m-6", ".m-8"],
          [".m-9"],
        ];

        animationOrder.forEach((targets, index) => {
          tl.to(
            targets.map(cls => img.querySelector(cls)),
            {
              clipPath: (i, el) => finalClipPaths[Array.from(masks).indexOf(el)],
              duration: 0.6,
              ease: "power2.out",
              stagger: 0.1,
            },
            index * 0.125
          );
        });
      });
    });
  });
}

// Llama a la función de configuración
setupAnimations();

 
  /* Panels */
  const cont = document.querySelector("#panels-container");
  const panels = gsap.utils.toArray("#panels-container .panel");

 
 

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
          end: () => "+=2000px",
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
          end: () => "+=2000px",
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
          end: () => "+=2000px",
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
          end: () => "+=2000px",
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
      return (window.innerWidth / logo.offsetWidth) * 205; // Cambiado a 0.5 para que no se agrande tanto  
    }  
    return 1; // Valor por defecto si no se obtiene el ancho  
  };  

  const timelineHeader = gsap.timeline({
  scrollTrigger: {
    trigger: "section.landing",
    start: "top top",
    end: "+=310%",  
    scrub: 1,
    pin: "#landing",
  },
});

timelineHeader.to(logo, {
  ease: "power2.inOut",
  scale: logoToScale,
  
  x: -2100,
});

const projects = document.querySelector("section.projects");  
// Asegúrate de que el final de la animación no cubra paneles
timelineHeader.to(projects, {
  ease: "power2.inOut",
  x: () => -(projects.scrollWidth - window.innerWidth),
  scrollTrigger: {
    trigger: projects,
    pin: true,
    start: 'center top',
    end: () => `+=${projects.scrollWidth - window.innerWidth}`,
    scrub: 1,
    invalidateOnRefresh: true,
  },
});

// Ajusta la animación de escala de 'methods' para evitar conflictos
timelineHeader.to(logo, {
  duration: 1.5,
  ease: "power2.inOut",
  scale: 0,
  scrollTrigger: {
    trigger: "#landing-2",
    start: "top top",
    scrub: 1,
    pin: true,
  },
});


  
});  


ctx = gsap.timeline();

ctx1 = gsap.timeline();


ctx1.to("#section-1", {
  opacity:1,
  delay:0.4,
  
})


ctx.to("#create1", {
  ease: "power4.out",

  y: "0%",
  
  opacity: 1,
  stagger: 4,
  duration: 1.1,
  delay: 1,
}),

gsap.to("#create1", {
  ease: "power4.out",

  y: "-13%",
  
  opacity: 1,
  stagger: 4,
  duration:1.8,
  delay: 1.6,

}),

gsap.to("#create2", {
    ease: "power3.out",
    y:"0%",
    opacity: 1,
    stagger: 4,
    duration: 1.1,
    delay: 0.9,
  });

 
gsap.to("#create2", {
  ease: "power4.out",

  y: "-13%",
  
  opacity: 1,
  stagger: 4,
  duration:1.8,
  delay: 1.6,

}),


gsap.to("#img1-k2p", {
  ease: "power3.inOut",
  x:"30%",
  duration: 0.8,

});


gsap.to("#img1-k2p", {
  ease: "power3.inOut",
  x:"-1%",
  y:"-4%",
  opacity: 0.9,
  duration: 0.8,
  delay: 2.6,

});


gsap.to("#paymentSolutions", {
  y:-40,
  opacity:1,
  duration:0.5,
  delay: 2.8,

})


gsap.to("#photo1", {
  x:-400,
  opacity:1,
  duration:1,
  scrub:true,
  scrollTrigger: {
    trigger:"#section-3",
    start:"-70px top",
  }
})


gsap.to("#photo2", {
  opacity:1,
  duration:1,
  scrub:true,
  scrollTrigger: {
    trigger:"#section-2",
    start:"top top",
  }
})

  
});

</script>

<template>
  <main> 
    
    <NavBar />
    <section id="section-1" class="section-1" >
             
             <div class="container1">
              <div class="makePayments">
                <div class="text-start" style=" z-index: 100 ;">
                  <h1>
                    <span
                      id="create1"
                      class="text-white blend-difference"
                    >
                      The best way
                    </span>
                  </h1>
                  <h1>
                    <span id="create2" class="text-white blend-difference">
                      To make payments
                    </span>
                  </h1>
      
                  <div id="paymentSolutions" class="paymentSolutions">
                    <div class="d-flex align-center text-white mr-10" style="margin-left: 0">
                      <span class="paymentTips mr-2" style="background-color:rgb(200, 211, 251,0.3); border-radius:24px; padding:5px ">
                        250+
                                
                      </span>
                      <span class="paymentTipsText mr-2">Payment </span>
                      <span class="paymentTipsText">Solutions</span>
                    </div>
      
                    <div class="d-flex align-center  text-white ">
                      <span class="paymentTips mr-2" style="background-color:rgb(200, 211, 251,0.3); border-radius:24px; padding:5px ">
                        100+
                                
                      </span>
                      <span class="paymentTipsText" >Countries</span>
                    </div>
                  </div>
                </div>
              </div>
              <div class="img-container">
                <img v-if="!$vuetify.display.mobile" id="img1-k2p"  decoding="async" width="500" height="400" src="https://key2pay.online/wp-content/uploads/2024/04/k2p_businessRecurso-1graphics_portada.png" class="attachment-full size-full wp-image-1884" alt="Tech" srcset="https://key2pay.online/wp-content/uploads/2024/04/k2p_businessRecurso-1graphics_portada.png 3562w, https://key2pay.online/wp-content/uploads/2024/04/k2p_businessRecurso-1graphics_portada-300x224.png 300w, https://key2pay.online/wp-content/uploads/2024/04/k2p_businessRecurso-1graphics_portada-1024x765.png 1024w, https://key2pay.online/wp-content/uploads/2024/04/k2p_businessRecurso-1graphics_portada-768x574.png 768w, https://key2pay.online/wp-content/uploads/2024/04/k2p_businessRecurso-1graphics_portada-1536x1148.png 1536w, https://key2pay.online/wp-content/uploads/2024/04/k2p_businessRecurso-1graphics_portada-2048x1531.png 2048w" style="opacity:0;"></img>
              </div>  
             </div>
       
       
    </section>
    <section id="section-2" class="section-2">
   
          <div class="container">
            <h2 v-if="!$vuetify.display.mobile" class="reveal-type">
              Our mission is to provide a <span style="color: #1597f5;">payment service</span>  <br> as efficiently as possible, to enhance  the  user <br> experience in  any <span style="color: #1597f5;">transaction</span>.
            </h2>
            <h2 v-else class="reveal-type">
              Our mission is to <br> provide a <span style="color: #1597f5;">payment service</span> <br>as efficiently as <br> possible, to enhance <br>  the  user <span style="color: #1597f5;">experience</span> in  <br> any <span style="color: #1597f5;">transaction</span>.
            </h2>
            <div v-if="$vuetify.display.mobile" style="width:100%; text-align:end; padding-top:100px">
              <img  id="photo2" src="https://key2pay.online/wp-content/uploads/2024/01/aurora_Recurso-1busi.png"  width="239" height="245" alt="" style="opacity: 0;">
            </div>
          </div>
       
    </section>
   
    <section id="section-3" class="section-3">
      <div id="info">
        <div class="info-left"  id="info-1">
          <div class="icon"><v-icon class="icon"icon="mdi-cogs" :color="$vuetify.display.mobile ? '#1597f5' : 'white'"  size="50px" ></v-icon></div>
          <div class="info-tittle">
             Easy to Setup
             <div class="info-para">
              Protect your customers' sensitive information with advanced encryption technology, ensuring every transaction is secure and worry-free.
             </div>
          </div>
        </div>
        <div class="info-right" id="info-2" >
          <div class="icon"><v-icon icon="mdi-lock"  :color="$vuetify.display.mobile ? '#1597f5' : 'white'"  size="50px" ></v-icon></div>
          <div class="info-tittle">
            Secure Transactions
             <div class="info-para">
              Protect your customers' sensitive information with advanced encryption technology, ensuring every transaction is secure and worry-free.
             </div>
          </div>
          
       </div>
       <div class="info-left" id="info-3">
        <div class="icon"><v-icon icon="mdi-map-marker"  :color="$vuetify.display.mobile ? '#1597f5' : 'white'"  size="50px" ></v-icon></div>
        <div class="info-tittle">
          Locals Payins & Payouts
           <div class="info-para">
            Process your payments with settlement of international, local, and/or hybrid funds.
           </div>
        </div>
        
     </div>
     <div class="info-right" id="info-4">
        <div class="icon"><v-icon icon="mdi-message-text" :color="$vuetify.display.mobile ? '#1597f5' : 'white'"  size="50px"></v-icon></div>
          <div class="info-tittle">
            Dedicated Merchant Support
            <div class="info-para">
              Rest easy knowing that our dedicated support team is available round-the-clock to assist you with any inquiries or issues you may encounter. 
            </div>
          </div>
        </div>
     </div>
     <div v-if="!$vuetify.display.mobile">
      <img  id="photo1" src="https://key2pay.online/wp-content/uploads/2024/01/aurora_Recurso-1busi.png"  width="399" height="385" alt="" style="position:absolute; bottom:60px; z-index:-1">
    </div>
    </section>
    <section style=" width:100%; height:auto; padding-top:40%" id="section-6">

     
         
      <div class="row">
          <div class="img img-7">  <h1>A Smart Solution for Your Payments</h1></div>
      </div>
      <div class="row">
        <div class="img img-1">
            
        </div>
        <div class="img img-2"></div>

  
       </div> 
       <div class="row">
        <div class="img img-3">
            
        </div>
        <div class="img img-4"></div>

  
       </div> 
       <div class="row">
        <div class="img img-5">
            
        </div>
        <div>
          
        </div>
        <div class="img img-6"></div>

  
       </div> 
      
     
     
    
   
  </section>
   
 
    
   
    <section id="panels" class="section-4">

      <div id="panels-container">
        <article id="panel-1" class="panel" >
          
        </article>
       
        <article id="panel-2" class="panel" >
          <div>
            <h3 class="worldPayments"> <span style="margin-right: 170px;"> </span> A whole world of payments</h3>
            
          </div>
          
        </article>
       
      
        
      </div>
     
    </section>
   
    <section  id="landing" class="sections  landing d-flex align-center justify-center " style="background-color: trasparent; height:100vh; " >
      <div class="text-center" >
        <h3 id="key" style="font-size: 22vw;color: white;  font-weight:500; z-index:2; padding-left:20px;">methods</h3>
      </div>

    </section>
    <div id="landing-2" class="sections" style="background-color: white; height:100vh; ">
      <section class="projects">
        
           <Map />
        
      </section>
   </div>
  



  
       


 
   


  </main>
</template>
