<template>
  <div class="home">

    &nbsp;
    <div class="card text-center" v-if="loginStatus">
            <div class="card-header">
                New Question
            </div>
            <div class="card-body">
              <wysiwyg id="formQuestion" v-model="question"/>
            </div>
            <div class="card-footer text-muted">
              <b-btn variant="primary" @click="submitQuestion">Submit new Questions</b-btn>  
            </div>
        </div>
    &nbsp;
    <div class="card text-center">
      <div class="card-header">
        All Questions
      </div>
      <div class="question-card" v-for="(question, index) in allQuestions" :key="index">
        <div class="card text-center">
          <div class="card-body">
            <p class="card-text">{{question.body}}</p>
            <a href="#" class="btn btn-primary">Go somewhere</a>
          </div>
          <div class="card-footer text-muted">
            by {{question.author.name}}
          </div>
        </div>
      </div>
      
    </div>
    
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'

import {mapState, mapActions} from 'vuex'

export default {
  name: 'home',
  data(){
    return {
      question:''
    }
  },
  components: {
    HelloWorld
  },
  computed: {
    ...mapState(['allQuestions', 'loginStatus', 'token'])
  },
  created(){
    this.$store.dispatch('getQuestions')
  },
  methods: {
    submitQuestion(){
      axios({
        method: 'post',
        url: 'http://localhost:3000/questions',
        data: {
          body: this.question
        }, 
        headers: {
          token: this.token
        }
      })
        .then(response=>{
          this.$store.dispatch('getQuestions')
        })
        .catch(err=>{
          console.log(err.errors)
        })
    }
  }
}
</script>

<style>
@import "~vue-wysiwyg/dist/vueWysiwyg.css";
#formQuestion{
  margin-top: 2%
}

.question-card{
  padding: 10px;
  border: 1px solid #e4e4e4;
}
</style>
