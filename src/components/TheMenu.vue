<template>
  <transition name="menu-transition" @after-enter="showMenuLinks = true">
    <div class="menu inset-0" v-if="menuIsShow">
      <div class="menu-box">
        <div
          class="close-icon"
          :class="close_icon_hover_effect"
          @mouseenter="mouse.close_icon_hover = true"
          @mouseleave="mouse.close_icon_hover = false"
          @click="closeMenu"
        >
          <img src="../assets/close.svg" alt />
        </div>
        <div class="menu-container">
          <ul
            class="menu-links"
            :class="{'menu-link-leave': leaving}"
            v-if="showMenuLinks"
            @click="closeMenu"
          >
            <!-- <transition name="menu-links-transition"  @after-leave="closeMenu"> -->
            <li v-for="link in links" :key="link.name">
              <router-link :to="link.path">
                <span>{{link.name}}</span>
              </router-link>
            </li>
            <!-- </transition> -->
          </ul>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  data: () => ({
    mouse: {
      close_icon_hover: false
    },
    showMenuLinks: false,
    leaving: false,
    links: [
      {
        path: "/",
        name: "HOME"
      }
      // {
      //   path: "photos",
      //   name: "PHOTO ALBUM"
      // },
      // {
      //   path: "countdown",
      //   name: "COUNTDOWN"
      // },
      // {
      //   path: "lovelife",
      //   name: "LOVE LIFE"
      // }
    ]
  }),
  computed: {
    close_icon_hover_effect() {
      return {
        hover: this.mouse.close_icon_hover
      };
    },
    menuIsShow: function() {
      return this.$store.state.menuIsShow;
    }
  },
  created() {
    // console.log(this.$refs.menuLink)
  },
  methods: {
    closeMenu() {
      this.leaving = true;
      setTimeout(() => {
        this.$store.commit("closeMenu");
        this.leaving = false;
        this.showMenuLinks = false;
      }, 1);
    }
  }
};
</script>

<style lang="scss" scoped>
.menu {
  position: fixed;
  background-color: #262626;
  z-index: 990;
  height: 100vh;
  color: #fff;
}
.menu-box {
  position: relative;
  width: 100vw;
  height: 100vh;
}
.close-icon {
  position: absolute;
  top: 57px;
  right: 70px;
  transition: transform 0.5s ease-out;
  &.hover {
    transform: rotate(-7deg);
  }
  img {
    width: 40px;
    animation: enter-rotate 1.5s ease-out;
  }
}
@keyframes enter-rotate {
  0% {
    transform: rotate(-20deg);
  }
  100% {
    transform: rotate(0deg);
  }
}
ul.menu-links {
  margin: auto;
  list-style: none;
  font-size: 4em;
  font-weight: 700;
  line-height: 0.95em;

  span {
    display: inline-block;
    transition: transform 0.6s ease-out;
    transform-origin: center left;
  }

  span:hover {
    transform: scale(1.1);
  }

  li {
    animation: fade-slide 0.4s ease-out both;
  }
}
@for $i from 1 through 4 {
  ul.menu-links li:nth-child(#{$i}n) {
    animation-delay: #{(0.15 * $i) - 0.15}s;
  }
}
@keyframes fade-slide {
  from {
    transform: translateY(30px);
    opacity: 0;
  }

  to {
    transform: translateY(0);
    opacity: 1;
  }
}

ul.menu-link-leave li {
  animation: fade-slide-leave 0.4s ease-out both;
}
@keyframes fade-slide-leave {
  from {
    transform: translateY(0);
    opacity: 1;
  }
  to {
    transform: translateY(30px);
    opacity: 0;
  }
}
@for $i from 1 through 4 {
  ul.menu-link-leave li:nth-child(#{$i}n) {
    animation-delay: #{($i * -0.15) + 0.45}s;
  }
}

.menu-container {
  // max-width: 1200px;
  height: 100vh;
  display: flex;
  flex-direction: column;
}
.menu-container h1 {
  font-size: 4em;
  margin: unset;
}

.menu-transition-enter-active {
  transition: transform 0.4s ease;
}
.menu-transition-leave-active {
  transition: transform 0.4s 0.55s ease;
}
.menu-transition-enter {
  transform: translateY(-100vh);
}
.menu-transition-enter-to,
.menu-transition-leave {
  transform: translateY(0);
}
.menu-transition-leave-to {
  transform: translateY(-100vh);
}

@media only screen and (max-width: 600px) {
  ul.menu-links {
    margin: auto 5px;
  }
  .close-icon {
    top: 20px;
    right: 25px;
  }
}
</style>

