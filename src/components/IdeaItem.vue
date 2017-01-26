<template>
  <article class="panel panel-default" :class="'idea-' + id" key="id">
    <div class="panel-heading title">
      <h4 class="panel-title static" :class="{hide: titleFocus}" @click="editIdeaClick('titleInput', 'title')">
        {{ title }}
        <span class="glyphicon glyphicon-remove" @click.self="deleteIdea(id)"></span>
      </h4>
      <input
        class="input-title not-static"
        :class="{hide: !titleFocus}"
        :value="title"
        v-on:blur="editIdeaBlur(id, 'title')"
        v-model="newTitle"
        ref="titleInput"
        autofocus />
    </div>
    <div class="panel-body body">
      <p 
        class="static"
        :class="{hide: bodyFocus}"
        @click="editIdeaClick('bodyInput', 'body')">
        {{ body }}
      </p>
      <textarea
        class="not-static"
        :class="{hide: !bodyFocus}"
        :maxlength="ideaLength"
        v-on:blur="editIdeaBlur(id, 'body')"
        v-model="newBody"
        ref="bodyInput"
        autofocus></textarea>
    </div>
    <small class="date">{{ outputDate(date) }}</small>
    <small class="ideaId">ID: {{ id }}</small>
  </article>
</template>

<script>
export default {
  name: 'IdeaItem',
  props: ['id', 'title', 'body', 'date', 'ideaLength', 'outputDate', 'editPost', 'deleteIdea'],
  data () {
    return {
      titleFocus: false,
      bodyFocus: false,
      newTitle: this.title,
      newBody: this.body
    }
  },
  methods: {
    editIdeaClick (focusPoint, type) {
      var i = this
      if (type === 'title') {
        i.titleFocus = !i.titleFocus
      } else if (type === 'body') {
        i.bodyFocus = !i.bodyFocus
      }
    },
    editIdeaBlur (id, type) {
      var i = this
      if (type === 'title') {
        i.titleFocus = !i.titleFocus
        i.editPost(id, type, i.newTitle)
      } else if (type === 'body') {
        i.bodyFocus = !i.bodyFocus
        i.editPost(id, type, i.newBody)
      }
    }
  }
}
</script>
<style>
  .panel { width: 150px; height: 150px; margin: 10px 0 10px 10px; float: left; overflow: hidden; position: relative; transition: all .3s ease .15s; opacity: 0.6 }
  .panel:hover,
    .panel:focus { opacity: 1 }
  .panel-heading { max-height: 25px; padding: 2px 10px 5px; overflow: hidden; position: relative }
  .panel-heading span { position: absolute; top: 6px; right: 3px; color: #999; cursor: pointer; opacity: 0; transition: all .3s ease .15s }
  .panel:hover .panel-heading span { opacity: 1 }
  .panel-heading span:hover { color: #8b0000 }
  .panel-heading input { width: 148px; margin-left: -10px }
  .panel-title { height: 20px; line-height: 1.3; cursor: pointer }
  .panel-body { max-height: 95px; padding: 10px 10px 0px; overflow: hidden; line-height: 1.5 }
  .panel-body p { height: 85px; margin: 0; padding: 0; cursor: pointer }
  .panel-body textarea { width: 100%; height: 80px }
</style>