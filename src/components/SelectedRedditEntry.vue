<template>
  <div v-if="entry !== null" class="entry">
    <h3 class="entry__title">{{ entry.title }}</h3>
    <div class="entry__image-wrapper" v-if="entry.image">
      <a :href="entry.image" target="_blank">
        <img class="entry__image" :src="image" alt="">
      </a>
    </div>
    <div class="entry__text" v-if="entry.text"> {{ text }} <a class="entry__text__show-more" @click="showFullText" v-if="text.length < entry.textLong.length && !showLongText">... show more</a></div>
    <div class="entry__text-wrapper">
      <div class="entry__comments">
        <img src="../assets/comment.svg" alt="" class="entry__comments__icon">
        <span>{{ entry.numComments }} comments</span>
      </div>
      <a @click="saveEntry" class="entry__save" v-show="!entry.saved">+ Save</a>
      <small v-show="entry.saved">&check; Saved</small>
      <small class="entry__author_date">
        Posted by {{ entry.author }} {{ entry.date | dateInDeltaTime }}
      </small>
    </div>
  </div>
</template>

<script>
export default {
  name: "SelectedRedditEntry",
  props: {
    entry: Object
  },
  data() {
    return {
      showLongText: false,
      loadingImage: 'data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHhtbG5zOnhsaW5rPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hsaW5rIiBzdHlsZT0ibWFyZ2luOmF1dG87YmFja2dyb3VuZDojZmZmO2Rpc3BsYXk6YmxvY2s7IiB3aWR0aD0iMjAwcHgiIGhlaWdodD0iMjAwcHgiIHZpZXdCb3g9IjAgMCAxMDAgMTAwIiBwcmVzZXJ2ZUFzcGVjdFJhdGlvPSJ4TWlkWU1pZCI+CiAgPGNpcmNsZSBjeD0iNTAiIGN5PSI1MCIgcj0iMzIiIHN0cm9rZS13aWR0aD0iOCIgc3Ryb2tlPSIjM0UzRTg5IiBzdHJva2UtZGFzaGFycmF5PSI1MC4yNjU0ODI0NTc0MzY2OSA1MC4yNjU0ODI0NTc0MzY2OSIgZmlsbD0ibm9uZSIgc3Ryb2tlLWxpbmVjYXA9InJvdW5kIiB0cmFuc2Zvcm09InJvdGF0ZSgyOTkuMjM4IDUwIDUwKSI+CiAgICA8YW5pbWF0ZVRyYW5zZm9ybSBhdHRyaWJ1dGVOYW1lPSJ0cmFuc2Zvcm0iIHR5cGU9InJvdGF0ZSIgcmVwZWF0Q291bnQ9ImluZGVmaW5pdGUiIGR1cj0iMS42MzkzNDQyNjIyOTUwODE4cyIga2V5VGltZXM9IjA7MSIgdmFsdWVzPSIwIDUwIDUwOzM2MCA1MCA1MCI+PC9hbmltYXRlVHJhbnNmb3JtPgogIDwvY2lyY2xlPgo8L3N2Zz4=',
      image: this.loadingImage
    }
  },
  computed: {
    text: function () {
      return this.showLongText ? this.entry.textLong : this.entry.text;
    }
  },
  methods: {
    showFullText() {
      this.showLongText = true;
    },
    saveEntry() {
      this.$emit('save-entry', this.entry);
    },
    loadImage() {
      let highResImage = new Image();
      highResImage.onload = function(){
        this.image = this.entry.image; // setting the attribute to image
      }.bind(this);
      highResImage.src = this.entry.image; // loading the image
    }
  },
  watch: {
    entry() {
      this.showLongText = false;
      this.image = this.loadingImage;
      this.loadImage();
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

  a {
    color: $color-pink;
    font-weight: 500;
    cursor: pointer;

    &:hover {
      text-decoration: underline;
      text-underline: deeppink;
    }
  }

  .entry {
    background-color: white;
    padding: 20px;
    overflow: auto;
    box-shadow: $color-gray-box-shadow 0px 0px 10px;
    margin-right: 20px;
    position: relative;
    box-sizing: border-box;
    display: flex;
    flex-direction: column;
    margin-top: 20px;
    margin-bottom: 20px;
    flex: 1;

    margin-left: 20px;

    @include media-breakpoint-up(md) {
      margin-left: 0;
    }

    &__title {
      font-size: 18px;
      text-align: left;
      margin-bottom: 18px;
    }

    &__image {
      max-width: 100%;
      max-height: 70vh;
    }

    &__image-wrapper {

    }

    &__text-wrapper {
      display: flex;
      margin-top: 20px;
      justify-content: space-between;
    }

    &__author_date {
      font-size: 14px;
    }

    &__comments {
      font-size: 14px;
      font-weight: 500;
      display: flex;


      &__icon {
        width: 18px;
        height: 18px;
        margin-right: 5px;
      }
    }

    &__text{
      word-break: break-word;
    }
  }
</style>
