<template>
  <div class="panel panel-default add-new" v-bind:class="{flip: tileFlip}">
    <article class="flipper">
      <div class="glyphicon glyphicon-plus front" @click="newIdeaItem"></div>
      <div class="back">
          <div class="panel-heading">
            <input placeholder="Idea title..." v-model="newTitleValue" class="input-title input-title-new" ref="inputTitle" />
          </div>
          <div class="panel-body">
            <textarea placeholder="Your idea..." v-model="newBodyValue" class="input-body-new" :maxlength="ideaLength" v-on:blur="attainNewIdeaValues"></textarea>
          </div>
          <small class="date">{{ outputDate(newIdeaDate) }}</small>
          <small class="ideaId">ID: {{ newIdeaId }}</small>
      </div>
    </article>
  </div>
</template>

<script>
export default {
  name: 'NewIdea',
  props: ['outputDate', 'ideaLength', 'newIdeaGet', 'newIdeaPost', 'newIdeaId', 'newIdeaDate'],
  data () {
    return {
      tileFlip: false,
      newTitleValue: '',
      newBodyValue: ''
    }
  },
  methods: {
    newIdeaItem () {
      var i = this
      i.tileFlip = true
      i.$refs.inputTitle.focus()
      i.newIdeaGet()
    },
    attainNewIdeaValues () {
      var i = this
      var prmTitle = i.newTitleValue
      var prmBody = i.newBodyValue
      i.tileFlip = false
      i.newTitleValue = ''
      i.newBodyValue = ''
      i.newIdeaPost(prmTitle, prmBody)
    }
  }
}
</script>
<style>
  .panel .glyphicon-plus { height: 148px; width: 148px; font-size: 50px; text-align: center; line-height: 2.8; cursor: pointer }
  .date { position: absolute; bottom: 5px; left: 10px }
  .ideaId { position: absolute; bottom: 5px; right: 10px }
  .add-new { perspective: 1000px }
  .add-new.flip .flipper { transform: rotateY(180deg) }
  .flipper { transition: 0.6s; transform-style: preserve-3d; }
  .front, .back { width: 100%; height: 148px; backface-visibility: hidden; position: absolute; top: 0; left: 0 }
  .front { z-index: 2; /* for firefox 31 */ transform: rotateY(0deg) }
  .back { transform: rotateY(180deg) }
  .back .panel-heading { margin: 0 10px; padding: 2px 0 5px }
  .back .panel-heading input { margin: 0 }
  .back input, .back textarea { border: 0 }
  .back input:focus, .back textarea:focus { border: initial }
</style>