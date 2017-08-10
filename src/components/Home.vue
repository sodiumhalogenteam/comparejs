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
            <p v-if="comparison.label" class="label">{{ comparison.label }}</p>
            <p>{{ comparison.start }}</p>
          </div>
          <div class="panel compare vertical-center">
            <div v-for="(option, i) in comparison.options">
              <p class="option" :data-c="comparison.chosen" :data-cc="comparisons[index].chosen" :data-i="i" :class="{active : comparison.chosen == i}" @click="trackSelection(index, i)">{{ option }}</p>
            </div>
          </div>
        </div>
      </div>
      <div class="carousel-cell">
        <h2>That's it!</h2>
        <p>To share your choices, please copy this link.</p>
        <p v-if="link">Share this link: <br><a :href="link">{{ link }}</a></p>
        <p v-else><button @click="generateLink()">Generate Link</button></p>
        <p>Maybe you would like to <a @click="startOver()">Go Back</a> to review?</p>
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
        label: 'Mission Statement',
        start: 'ATA is dedicated to the implementation of strategies that enhance the success of our clients and people.',
        options: [
          'ATA provides strategies that enhance the success of our clients and people.',
          'ATA implements sound strategies that enhance the success of our clients and people.',
          'Using (proven) strategies, ATA enhances the success of our clients and people',
          'We think strategically and improve our customer\'s success',
          '[nope, the original wins]'
        ]
      }, {
        type: 'text',
        label: 'Vision',
        start: 'ATA will be the premier source of accounting and advisory services helping clients achieve their goals.',
        options: [
          'ATA is the source of accounting and advisory services helping clients achieve their goals.',
          '[nope, the original wins]'
        ]
      }, {
        type: 'text',
        label: 'Strategic Objectives',
        start: 'Enhance the success of the firm and its employees.',
        options: [
          'Enhance the success of the firm and its employees.',
          'Enhance the success of the firm and its employees.',
          '[nope, the original wins]'
        ]
      }, {
        type: 'text',
        label: 'Strategic Objectives',
        start: 'Leverage the firm’s technology to provide a competitive advantage.',
        options: [
          'Leverage the firm’s technology to provide a competitive advantage.',
          'Leverage the firm’s technology to provide a competitive advantage.',
          '[nope, the original wins]'
        ]
      }, {
        type: 'text',
        label: 'Strategic Objectives',
        start: 'Enhance the firm’s talent development program to hire and retain quality personnel.',
        options: [
          'Enhance the firm’s talent development program to hire and retain quality personnel.',
          'Enhance the firm’s talent development program to hire and retain quality personnel.',
          '[nope, the original wins]'
        ]
      }, {
        type: 'text',
        label: 'Strategic Objectives',
        start: 'Promote the one-firm concept (Shared Vision) and develop a firm culture based on teamwork.',
        options: [
          'Promote the one-firm concept (Shared Vision) and develop a firm culture based on teamwork.',
          'Promote the one-firm concept (Shared Vision) and develop a firm culture based on teamwork.',
          '[nope, the original wins]'
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
      this.link = null
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
        this.comparisons[i].chosen = Number(choices[i])
      }
    },
    generateLink () {
      let url = location.protocol + '//' + location.host + location.pathname + '?choices='
      for (let i = 0; i < this.comparisons.length; i++) {
        let total = this.comparisons.length - 1
        if (total === i) {
          url += this.comparisons[i].chosen
        } else {
          url += this.comparisons[i].chosen + '+'
        }
      }
      this.link = url
    }
  },
  mounted () {
    for (let i = 0; i < this.comparisons.length; i++) {
      this.comparisons[i].chosen = null
    }
    let choices = this.getUrlVar('choices')
    if (choices) {
      this.addOptionsToComparison(choices)
    }
    this.$forceUpdate()
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
    p.label {
      font-size: 18px;
      font-weight: 700;
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
