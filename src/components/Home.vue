<template>
  <div class="home wrap">
    <flickity ref="flickity" class="carousel" :options="flickityOptions">
      <div class="carousel-cell">
        <h2>Let's begin comparing the mission statement options.</h2>
        <button @click="next()">Okay, I'm ready!</button>
      </div>
      <div v-for="(comparison, index) in comparisons">
        <div class="carousel-cell in-wrap">
          <div class="panel start">
            <p class="heading">original</p>
          </div>
          <div class="panel compare">
            <p class="heading">Are any of these better?<br>select a better option</p>
          </div>
          <div class="panel start vertical-center">
            <p>{{ comparison.start }}</p>
          </div>
          <div class="panel compare vertical-center">
            <div v-for="(option, i) in comparison.options">
              <p class="option" :class="{active : comparison.chosen === i}" @click="trackSelection(index, i)">{{ option }}</p>
            </div>
          </div>
        </div>
      </div>
      <div class="carousel-cell">
        <h2>That's it!</h2>
        <p>To share your choices, please copy this link.</p>
        <p v-if="link"><a :href="link">{{ link }}</a></p>
        <p v-else>Share this link: <br><button @click="generateLink()">Generate Link</button></p>
        <p>Maybe you would like to <a @click="startOver()">Go Back</a> to review ?</p>
      </div>
    </flickity>
  </div>
</template>

<script>
import Flickity from 'vue-flickity'

export default {
  name: 'home',
  components: { Flickity },
  data () {
    return {
      link: null,
      comparisons: [{
        type: 'text',
        start: 'Today is going to be the worst!',
        options: [
          'Today is going to be great!',
          'Momma, said there\'d be days like this.',
          'I\'m great! You\'re great! Everything is great!'
        ]
      }, {
        type: 'text',
        start: 'Work harder people!',
        options: [
          'Go big. Win big.',
          'If there is time to lean there is time to clean',
          'Poop or get off the pot.'
        ]
      }],
      flickityOptions: {
        initialIndex: 1,
        prevNextButtons: false,
        pageDots: true,
        accessibility: false,
        setGallerySize: false,
        percentPosition: false,
        draggable: false
      }
    }
  },
  watch: {
    // whenever question changes, this function will run
    question: function (newQuestion) {
      this.answer = 'Waiting for you to stop typing...'
      this.getAnswer()
    }
  },
  methods: {
    trackSelection (comparison, option) {
      this.comparisons[comparison].chosen = option
      this.$forceUpdate()
      this.$refs.flickity.next()
    },
    next () {
      this.$refs.flickity.next()
    },
    previous () {
      this.$refs.flickity.previous()
    },
    startOver () {
      this.$refs.flickity.select(0, false, true)
      this.link = null
    },
    getUrlVar (q) {
      return (window.location.search.match(new RegExp('[?&]' + q + '=([^&]+)')) || [null])[1]
    },
    addOptionsToComparison (choices) {
      // clean up options from URL variable fetching
      choices = choices.split('+')
      for (let i = 0; i < this.comparisons.length; i++) {
        this.comparisons[i].chosen = Number(choices[i]) || null
      }
    },
    generateLink () {
      let url = location.protocol + '//' + location.host + location.pathname + '?choices='
      for (let i = 0; i < this.comparisons.length; i++) {
        url += this.comparisons[i].chosen + '+'
      }
      this.link = url
    }
  },
  mounted () {
    for (let i = 0; i < this.comparisons.length; i++) {
      this.comparisons[i].options.push('[nope, the original wins]')
      this.comparisons[i].chosen = null
    }
    let choices = this.getUrlVar('choices')
    if (choices) {
      this.addOptionsToComparison(choices)
    }
    this.startOver()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
@import url('../../node_modules/flickity/dist/flickity.min.css');
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
  &:hover {
    cursor: pointer;
  }
}
a.btn, button {
  padding: 12px;
  background: #42b983;
  color: #fff;
  text-decoration: none;
  font-size: 18px;
  outline: none;
  border: 0;
  &:hover, &:focus, &:active {
    background: darken(#42b983, 10%);
    cursor: pointer;
  }
}
.wrap {
  .panel {
    position: relative;
    padding: 50px;
    width: 40%;
    .in-wrap {
      position: relative;
    }
    &.start {
      float: left;
      p {
        font-size: 2.7em;
      }
    }
    &.compare {
      float: right;
      p.option {
        font-size: 25px;
        padding: 0 0 3%;
        border-bottom: 1px solid #eee;
        &:hover {
          color: #42b983;
          border-bottom: 1px solid lighten(#42b983, 30%);
          cursor: pointer;
          font-weight: 700;
        }
        &.active {
          color: #42b983;
          border-bottom: 1px solid #42b983;
          font-weight: 700;
        }
      }
    }
    p.heading {
      text-align: center;
      font-size: 18px;
      color: #a9a9a9;
      position: absolute;
      top: 0;
      left: 30%;
      right: 30%;
      width: 39.9%;
    }
  }
}
.vertical-center{
  position: absolute;
  top:50%;
  left: 0%;
  transform:translate(0%, -50%);
  -webkit-transform:translate(0%, -50%);
}
/* inherit height from parent */
.carousel { height: 80vh; }
.carousel-cell { width: 100vw; height: 50vh; }
/* move page dots into carousel */
.flickity-page-dots { bottom: 10%; }
</style>
