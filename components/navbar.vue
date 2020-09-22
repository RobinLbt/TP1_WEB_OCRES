<template>
  <div id="navbar">
    <section id="gauche" v-if="!isMobile">
      <transition appear name="scalein2" mode="in-out">
        <!-- <img src="@/assets/logo.svg" id="logo" /> -->
        <p>LOGO ICI</p>
      </transition>
    </section>

    <section id="droite">
      <button v-if="isClicked" @click="isClicked = false">🞬</button>
      <button v-if="!isClicked" @click="isClicked = true">☰</button>
      <router-link
        v-if="isMobile == true ? isClicked : !isMobile"
        v-on:click.native="isClicked=false"
        to="/apropos"
        class="link"
      >À propos</router-link>
      <router-link
        v-if="isMobile == true ? isClicked : !isMobile"
        v-on:click.native="isClicked=false"
        to="/contact"
        class="link"
      >Mes travaux</router-link>

      <img src="~/assets/icons/linkedin.svg" />
      <img src="~/assets/icons/mail.svg" />
    </section>
  </div>
</template>

<script>
import { gsap } from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";

if (process.client) {
  gsap.registerPlugin(ScrollTrigger);
}

export default {
  data() {
    return {
      isClicked: false,
      isMobile: false,
    };
  },
  methods: {
    onResize() {
      if (window.innerWidth <= 800) {
        this.isMobile = true;
      } else {
        this.isMobile = false;
      }
    },
  },
  mounted() {
    if (process.client) {
      gsap.to("#navbar", {
        scrollTrigger: {
          trigger: "#landingPhoto",
          markers: true,
          start: "=+200",
          end: "center top",
          toggleActions: "restart pause reverse pause",
        },
        backgroundColor: "#FFFFFF",
        // triggerActions: "restart pause reverse pause"
      });
    }
  },
  created() {
    window.addEventListener("resize", this.onResize);
  },

  beforeDestroy() {
    window.removeEventListener("resize", this.onResize);
  },
};
</script>
<style>
#navbar {
  display: flex;
  flex-direction: row;
  width: 100%;
  height: fit-content;
  align-items: center;
  font-size: 1.5rem;
  font-family: 'geomanistregular';
  font-weight: 500;
  position: fixed;
  padding: 0;
  margin: 0;
}

#gauche {
  display: flex;
  justify-content: flex-start;
  flex: 1;
}

#droite {
  flex: 2;
  display: flex;
  flex-direction: row;
  justify-content: space-evenly;
  align-items: center;
  height: 100%;
}

.link {
  color: #707070;
  padding-left: 0.8rem;
  padding-right: 0.8rem;
  padding-top: 0.8rem;
  padding-bottom: 0.8rem;
  outline: 0;
}

.link:hover {
  color: #835b91;
}

#logo {
  width: 15rem;
  transition-timing-function: ease;
  transition-duration: 0.65s;
  transition-delay: 0.0444444s;
}

.router-link-active {
  color: #835b91;
  border-bottom: solid #835b91 0.2rem;
}

button {
  display: none;
  width: 100%;
}

@media only screen and (max-width: 800px) {
  #navbar {
    -webkit-box-shadow: none;
    -moz-box-shadow: none;
    box-shadow: none;
  }

  #droite {
    flex-direction: column;
    align-items: center;
  }
  button {
    display: block;
    padding: 2vw;
    font-size: 3vh;
    background-color: none;
    border: none;
    outline: none;
    cursor: pointer;
  }
  .link {
    background-color: whitesmoke;
    width: 100%;
  }
  .router-link-active {
    background-color: #e5edff;
    border: none;
    font-weight: bold;
  }
}
</style>
