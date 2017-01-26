<template>
    <section class="sort-page">
        <select class="form-control" v-model="selected">
            <option :value="sortPlaceHolderValue">{{sortPlaceHolder}}</option>
            <option :value="sortOne">{{sortOneText}}</option>
            <option :value="sortTwo">{{sortTwoText}}</option>
        </select>
    </section>
</template>

<script>
export default {
  name: 'SortIdeas',
  props: ['setSort'],
  data () {
    return {
      sortPlaceHolderValue: '',
      sortOne: 'title',
      sortTwo: 'created_date',
      sortPlaceHolder: 'Sort your ideas',
      sortOneText: 'By title',
      sortTwoText: 'By date created',
      selected: ''
    }
  },
  watch: {
    selected () {
      this.setSort(this.selected)
    }
  }
}
</script>

<style>
    .sort-page { margin-top: 10px }
</style>