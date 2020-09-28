<template>
  <div>
    <h1>Quiz</h1>
    <b-nav tabs class="d-flex justify-content-end">
      <b-nav-item>
          <b-form-select v-model="selected" :options="options"></b-form-select>
      </b-nav-item>
      <b-nav-item disable><label :class="color">Counter: {{ numCorrect }}/10</label> </b-nav-item>
      <b-nav-item>Answered Questions: {{numTotal}}</b-nav-item>
    </b-nav>
  </div>
</template>

<script>
export default {
  props: ["numCorrect", "numTotal"],
  data() {
      return {
        selected: null,
        options: [
          { value: null, text: 'Choose difficulty' },
          { value: 'easy', text: 'Easy' },
          { value: 'medium', text: 'Medium' },
          { value: 'hard', text: 'Hard'}
        ]
      }
    },
    watch:{
        selected: function(){
            this.$emit("select-difficulty", this.selected);
        }
    },
    computed: {
      color(){
        return{
          'text-danger': this.numCorrect < 5,
          'text-warning': this.numCorrect >= 5 && this.numCorrect <= 7,
          'text-success': this.numCorrect >= 8
        }
      }
    }
};
</script>