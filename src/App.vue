<template>
  <div id="app" class="container">
    <SortIdeas :setSort="setSortKey"></SortIdeas>
    <section class="outer">
      <IdeaItem
        v-for="(idea, index) in results"
        :id="idea.id"
        :title="idea.title"
        :body="idea.body"
        :date="idea.created_date"
        :ideaLength="maxLength"
        :outputDate="dateRender"
        :editPost="ideaEditPost"
        :deleteIdea="deleteIdea">
      </IdeaItem>
      <NewIdea
        :ideaLength="maxLength"
        :outputDate="dateRender"
        :newIdeaGet="newIdea"
        :newIdeaPost="newIdeaComplete"
        :newIdeaId="ideaNewId"
        :newIdeaDate="ideaNewDate">
      </NewIdea>
    </section>
  </div>
</template>

<script>
import 'bootstrap/dist/css/bootstrap.css'
import axios from 'axios'
import _ from 'lodash'
import SortIdeas from './components/Sorting'
import IdeaItem from './components/IdeaItem'
import NewIdea from './components/NewIdea'
export default {
  name: 'app',
  data () {
    return {
      results: {},
      ideas: 'http://localhost:3000/ideas',
      ideaNew: 'http://localhost:3000/ideas/new',
      ideaEdit: 'http://localhost:3000/idea/update',
      ideaDelete: 'http://localhost:3000/idea/delete',
      maxLength: 140,
      sortOrder: '',
      ideaNewId: '',
      ideaNewDate: ''
    }
  },
  methods: {
    getIdeas () {
      var i = this
      axios.get(i.ideas, {
        timeout: 5000
      }).then((response) => {
        i.results = response.data
        i.results = _.orderBy(i.results, i.sortOrder)
      })
    },
    ideaEditPost (ideaId, ideaEditField, ideaEditValue) {
      axios.post(this.ideaEdit, {
        id: ideaId,
        [ideaEditField]: ideaEditValue
      }).then((response) => {
        this.getIdeas()
      })
    },
    newIdea () {
      var i = this
      axios.get(i.ideaNew).then((response) => {
        response = response.data
        i.ideaNewId = response.id
        i.ideaNewDate = response.created_date
      })
    },
    newIdeaComplete (title, body) {
      var i = this
      axios.post(i.ideaEdit, {
        id: i.ideaNewId,
        created_date: i.ideaNewDate,
        title,
        body
      }).then((response) => {
        i.ideaNewIdea = ''
        i.ideaNewDate = ''
        this.getIdeas()
      })
    },
    deleteIdea (deletedId) {
      var i = this
      axios.post(i.ideaDelete, {
        id: deletedId
      }).then((response) => {
        i.getIdeas()
      })
    },
    setSortKey (value) {
      var i = this
      i.sortOrder = value
      i.results = _.orderBy(i.results, i.sortOrder)
    },
    dateRender (newDate) {
      var months = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec']
      var date = new Date(newDate)
      var day = date.getDate()
      var monthIndex = date.getMonth()
      var month = months[monthIndex]
      var year = date.getFullYear()
      var articleDate = day + ' ' + month + ' ' + year
      return articleDate
    }
  },
  components: {
    SortIdeas,
    IdeaItem,
    NewIdea
  },
  mounted () {
    this.getIdeas()
  }
}
</script>

<style>
  #app { max-width: 980px; margin: 0 auto; }
  .outer { margin-left: -10px; overflow: hidden }
  input,
    textarea { outline: none; border: none ; -webkit-box-shadow: none !important; -moz-box-shadow: none !important; box-shadow: none !important; }
  input:focus, 
    textarea:focus { border: solid 1px #999!important }
</style>


