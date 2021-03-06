<template>
  <div class="knowledge-header inline">
    <h1>
      {{ $t('me') }} {{ $t(currentUtil.language ? 'develop' : 'use') }}
      <br v-if="currentUtil.name.length > 4" class="sm:hidden">
      <span ref="util" class="magic-text bg-gradient-to-r bg-clip-text text-transparent" :style="{'--tw-gradient-stops': bgUtilClasses}">
        {{ currentUtil.name }}
      </span>
      <br>
      <span v-if="currentFramework">
        {{ $t('and') }}
        <a
          ref="framework"
          rel="noopener"
          class="group magic-text framework-link bg-gradient-to-r bg-clip-text text-transparent"
          target="_blank"
          :style="{'--tw-gradient-stops': bgFrameworkClasses}"
          :href="currentFramework.url"
        >
          {{ currentFramework.name }}
          <ExternalLinkIcon
            class="link-icon opacity-0 transition-opacity group-hover:opacity-60 dark:group-hover:opacity-60 duration-300 ease-in-out hidden sm:inline w-6 h-6 md:w-12 h-12 -ml-1 md:-ml-2 -mt-2"
            :style="{color: linkColor}"
          />
        </a>
      </span>
    </h1>
  </div>
</template>

<script>
import colorTheme from 'tailwindcss/colors'
import { ExternalLinkIcon } from '@vue-hero-icons/outline'

export default {
  name: 'KnowledgeHeader',
  components: { ExternalLinkIcon },
  data () {
    return {
      timer: null,
      currentUtilIndex: 0,
      currentFrameworkIndex: 0,
      utilities: [
        {
          name: 'Javascript',
          colors: ['yellow', 'yellow'],
          language: true,
          frameworks: [
            {
              name: 'Vue.js',
              url: 'https://vuejs.org/',
              colors: ['green', 'green']
            },
            {
              name: 'React',
              url: 'https://reactjs.org/',
              colors: ['blue', 'blue']
            },
            {
              name: 'JQuery',
              url: 'https://jquery.com/',
              colors: ['blue', 'blue']
            },
            {
              name: 'NodeJS',
              url: 'https://nodejs.org/',
              colors: ['green', 'green']
            }
          ]
        },
        {
          name: 'CSS',
          colors: ['blue', 'indigo'],
          language: true,
          frameworks: [
            {
              name: 'Sass',
              url: 'https://sass-lang.com/',
              colors: ['red', 'pink']
            },
            {
              name: 'Bootstrap',
              url: 'https://getbootstrap.com/',
              colors: ['purple', 'indigo']
            },
            {
              name: 'Tailwind',
              url: 'https://tailwindcss.com/',
              colors: ['teal', 'teal']
            }
          ]
        },
        {
          name: 'PHP',
          colors: ['indigo', 'purple'],
          language: true,
          frameworks: [
            {
              name: 'Laravel',
              url: 'https://laravel.com/',
              colors: ['red', 'red']
            },
            {
              name: 'Wordpress',
              url: 'https://wordpress.org/',
              colors: ['gray', 'gray']
            }
          ]
        },
        {
          name: 'Java',
          colors: ['red', 'blue'],
          language: true,
          frameworks: [
            {
              name: 'Netty',
              url: 'https://netty.io/',
              colors: ['gray', 'blue']
            },
            {
              name: 'SpigotMC',
              url: 'https://www.spigotmc.org/',
              colors: ['orange', 'gray']
            }
          ]
        },
        {
          name: 'MySQL',
          colors: ['orange', 'orange'],
          language: false,
          frameworks: null
        },
        {
          name: 'MongoDB',
          colors: ['green', 'green'],
          language: false,
          frameworks: null
        },
        {
          name: 'Cloud Computing',
          colors: ['red', 'pink'],
          language: true,
          frameworks: [
            {
              name: 'AWS',
              url: 'https://aws.amazon.com/',
              colors: ['orange', 'yellow']
            },
            {
              name: 'Google Cloud',
              url: 'https://cloud.google.com/',
              colors: ['blue', 'yellow']
            },
            {
              name: 'Firebase',
              url: 'https://firebase.google.com/',
              colors: ['amber', 'orange']
            }
          ]
        }
      ]
    }
  },
  computed: {
    currentUtil () {
      return this.utilities[this.currentUtilIndex]
    },
    currentFramework () {
      return this.currentUtil.frameworks != null ? this.currentUtil.frameworks[this.currentFrameworkIndex] : null
    },
    bgUtilClasses () {
      return this.bgClasses(this.currentUtil.colors)
    },
    bgFrameworkClasses () {
      return this.bgClasses(this.currentFramework.colors)
    },
    linkColor () {
      const variant = this.$colorMode.value === 'light' ? 600 : 400
      return colorTheme[this.currentFramework.colors[0]][variant]
    }
  },
  mounted () {
    this.timer = setInterval(this.updateText, 5000)
  },
  beforeDestroy () {
    clearInterval(this.timer)
  },
  methods: {
    bgClasses (colors) {
      return `${colorTheme[colors[0]][400]}, ${colorTheme[colors[1]][600]}, transparent, transparent`
    },
    updateText () {
      if (
        this.currentUtil.frameworks == null ||
        (this.currentUtil.frameworks.length - 1) <= this.currentFrameworkIndex
      ) {
        if (this.currentFramework) {
          this.$refs.framework.classList.add('fade-out')

          setTimeout(() => {
            this.$refs.framework.classList.remove('fade-out')
            this.currentFrameworkIndex = 0
          }, 1250)
        }

        this.$refs.util.classList.add('fade-out')
        setTimeout(() => {
          this.$refs.util.classList.remove('fade-out')

          if (this.currentUtilIndex >= this.utilities.length - 1) {
            this.currentUtilIndex = 0
          } else {
            this.currentUtilIndex++
          }
        }, 1250)
      } else {
        this.$refs.framework.classList.add('fade-out')
        setTimeout(() => {
          this.$refs.framework.classList.remove('fade-out')
          this.currentFrameworkIndex++
        }, 1250)
      }
    }
  }
}
</script>

<style scoped>
.knowledge-header {
  min-height: 96px;
}
.magic-text {
  @apply opacity-100 m-0 relative z-10;
  transition: background-position 1.5s ease-in-out, opacity 1.5s cubic-bezier(1,0,1,.5);
  background-size: 350%;
}
.fade-out {
  @apply opacity-0;
  background-position: 100%;
}
</style>
