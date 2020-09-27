<template>
  <div>
    <client-only>
      <navbar></navbar>
    </client-only>

    <section id="parralaxLanding">
      <img id="namibiaCiel" class="parralaximg" src="~assets/images/namibia_ciel.png" />
  
    <section id="textLanding">
      <p id="hello" >{{ bonjour }}</p>
      <p id="iam" >Je suis Robin, ingénieur de formation</p>
    </section>
      <img id="namibiaDunes" class="parralaximg" src="~assets/images/namibia_dunes.png" />
    <img
        src="~/assets/icons/arrowdown.svg"
        width="80rem"
        style="bottom: 1rem; position: absolute"
        id="arrowDown"
      />
    </section>

    <description class="parralaxSection"></description>

    
    <div style="width: 100%; height: 120rem; background-color: #fcfcfc"></div>
    //We are all connected ! avec mains qui tiennent smarphone qui apparaissent
    sur le côté //Je suis ingé en IoT // mais plus encore .. //mettre cloud of
    words de tout ce que je fais
  </div>
</template>

<script>
import navbar from "~/components/navbar";
import description from "~/components/descriptionSection";

import { gsap } from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";

if (process.client) {
  gsap.registerPlugin(ScrollTrigger);
}

export default {
  components: { navbar },
  data() {
    return {
      bonjour: "Bonjour",
    };
  },
  methods: {
    randomBonjour() {
      let differentHello = [
        "Bonjour",
        "Holà",
        "Hello",
        "Hallo",
        "Buongiorno",
        "Olá",
        "Привет",
        "صباح الخير",
        "你好",
        "こんにちは",
        "여보세요",
        "Hallå",
        "Hej",
        "สวัสดี",
        "Dzień dobry",
        "שלום",
        "Zdravo",
        "Salve",
        "Merhaba",
        "Hei",
        "Xin chào",
        "Mholo",
      ];
      let message = "";
      do {
        message =
          differentHello[Math.floor(Math.random() * differentHello.length)];
      } while (message == this.bonjour);
      return message;
    },
  },

  mounted() {
    if (process.client) {
      //! Effet jelly sur certains elements HTML quand scroll
      let proxy = { skew: 0 },
        skewSetter = gsap.quickSetter(".skewElem", "skewY", "deg"), // fast
        clamp = gsap.utils.clamp(-10, 10); // don't let the skew go beyond 10 degrees.

      ScrollTrigger.create({
        onUpdate: (self) => {
          let skew = clamp(self.getVelocity() / -300);
          // only do something if the skew is MORE severe. Remember, we're always tweening back to 0, so if the user slows their scrolling quickly, it's more natural to just let the tween handle that smoothly rather than jumping to the smaller skew.
          if (Math.abs(skew) > Math.abs(proxy.skew)) {
            proxy.skew = skew;
            gsap.to(proxy, {
              skew: 0,
              duration: 0.8,
              ease: "power3",
              overwrite: true,
              onUpdate: () => skewSetter(proxy.skew),
            });
          }
        },
      });

      // make the right edge "stick" to the scroll bar. force3D: true improves performance
      gsap.set(".skewElem", { transformOrigin: "right center", force3D: true });

      //!Timeline principale contenant toutes les animations
      var tl = gsap.timeline();
      tl.to("#arrowDown", 0.6, { y: 10, repeat: -1, yoyo: true }) //?Fleche qui bounce
        .from("#hello", {
          //Texte hello
          scrollTrigger: {
            trigger: "#textLanding",
            toggleActions: "restart none none reverse",
          },
          opacity: 0,
          y: "-10rem",
          z: "-10rem",
          rotationZ: "-10rem",
          transformOrigin: "right top",
          duration: 1.2,
          ease: "true",
          // onComplete: () => this.randomBonjour()
        })
        .from("#iam", {
          //?Texte je suis robin
          scrollTrigger: {
            trigger: "#textLanding",
            // start: "=+500",
            toggleActions: "restart none none reverse",
          },
          // "=+400",
          opacity: 0,
          x: "-5rem",
          duration: 0.64,
          ease: "true",
          onStart: () =>
            setInterval(() => {
              let message = this.randomBonjour();
              this.bonjour = message;
            }, 2000),
        })
        .from("#arrowDown", {
          //?Arrivée fleche qui bounce
          scrollTrigger: {
            trigger: "#textLanding",
            toggleActions: "restart none none reverse",
          },
          delay: 2,
          opacity: 0,
          ease: "true",
        })
        .to("#arrowDown", {
          //?Disparition fleche qui bounce
          scrollTrigger: {
            markers: true,
            trigger: "#textLanding",
            start: "=- 200",
            toggleActions: "restart none none reverse",
          },
          opacity: 0,
          duration: 1.4,
          ease: "true",
        }) 
        .to("#namibiaDunes", {
          yPercent: -20,
          ease: "none",
          scrollTrigger: {
            trigger: "#parralaxLanding",
            // start: "top bottom", // the default values
            // end: "bottom top",
            scrub: true,
            pin: true,
          },
        })
        .to("#textLanding", {
          yPercent: 30,
          ease: "none",
          scrollTrigger: {
            trigger: "#textLanding",
            // start: "top bottom", // the default values
            // end: "bottom top",
            scrub: true,
            pin: true,
          },
        })
     
        .to(".parralaxSection", {
          yPercent: 50,
          ease: "none",
          scrollTrigger: {
            trigger: "#textLanding",
            // start: "top bottom", // the default values
            // end: "bottom top",
            scrub: true,
            pin: true,
          },
        });
    }
  },
};
</script>

<style>

body {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
  background-color: #f9f9f9;
}

html {
  margin: 0;
  padding: 0;
  overflow: scroll;
  width: 100%;
  height: 100%;
  overflow-x: hidden;
}

a {
  text-decoration: none;
}

#footer {
  margin-top: 20rem;
  margin-bottom: 2rem;
}

#hello {
  font-family: "geomanistregular";
  color: whitesmoke;
  font-size: 8rem;
  margin: 0;
  text-align: center;
}

#iam {
  font-family: "geomanistregular";
  color: whitesmoke;
  font-size: 3rem;
  margin: 0;
  margin-bottom: 20rem;
  text-align: center;
}

#parralaxLanding {
  position: relative;
    height: 100vh;
  width: 100%;
  left: 0px;
  top: 0px;
  overflow: hidden;
  display: flex;
  justify-content: center;
  align-items: center;
}

.parralaximg {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  pointer-events: none;
  overflow: hidden;
  /* border: red 1rem solid; */
}

#textLanding {
  position: absolute;
  /* background-image: url("~assets/images/namibia.jpg"); */
  height: 100%;
  width: 100%;
  left: 0px;
  top: 0px;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

@font-face {
  font-family: "geomanistregular";
  src: url("../assets/fonts/regular/geomanist-regular-webfont.eot");
  src: url("../assets/fonts/regular/geomanist-regular-webfont.eot?#iefix")
      format("embedded-opentype"),
    url("../assets/fonts/regular/geomanist-regular-webfont.woff2")
      format("woff2"),
    url("../assets/fonts/regular/geomanist-regular-webfont.woff") format("woff"),
    url("../assets/fonts/regular/geomanist-regular-webfont.ttf")
      format("truetype"),
    url("../assets/fonts/regular/geomanist-regular-webfont.svg#geomanistregular")
      format("svg");
  font-weight: normal;
  font-style: normal;
}

@font-face {
  font-family: "geomanistregular";
  src: url("../assets/fonts/medium/geomanist-medium-webfont.eot");
  src: url("../assets/fonts/medium/geomanist-medium-webfont.eot?#iefix")
      format("embedded-opentype"),
    url("../assets/fonts/medium/geomanist-medium-webfont.woff2") format("woff2"),
    url("../assets/fonts/medium/geomanist-medium-webfont.woff") format("woff"),
    url("../assets/fonts/medium/geomanist-medium-webfont.ttf")
      format("truetype"),
    url("../assets/fonts/medium/geomanist-medium-webfont.svg#geomanistregular")
      format("svg");
  font-weight: 500;
  font-style: normal;
}
</style>