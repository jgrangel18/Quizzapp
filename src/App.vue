<template>
	<div id="app">
		<Header
		v-bind:numCorrect="numCorrect"
		v-bind:totalCorrect="totalCorrect"
		/>
    
    <br>
		<b-container border v-if="questionary">
      <h2>Please create your Quiz</h2>
			<b-form @submit.prevent="handleSubmit">
        <b-form-group class="form-row" >
          <label class="col-lg-4 col-form-label"  for="formQuiz.selectedNumber">Number of Questions</label>
          <b-form-select  class="col-lg-2" v-model="formQuiz.selectedNumber" :options="formQuiz.numberQuestions"></b-form-select>
        </b-form-group>
        <b-form-group  class="form-row">
          <label class="col-lg-4 col-form-label"  for="formQuiz.categories">Choose the category you want to try!!</label>
          <b-form-select  class="col-lg-2" v-model="formQuiz.selectedCategory" :options="formQuiz.categories"></b-form-select>
        </b-form-group>
        <b-form-group  class="form-row">
          <label class="col-lg-4 col-form-label"  for="formQuiz.selectedDifficulty">Choose the level fo difficulty!!</label>
          <b-form-select  class="col-lg-2" v-model="formQuiz.selectedDifficulty" :options="formQuiz.difficulty"></b-form-select>
        </b-form-group>
        <b-button type="submit" variant="primary">Submit</b-button>
        <b-button type="reset" variant="danger">Reset</b-button>
        <br>
        <br>
			</b-form>


		</b-container>
		<b-container class="bv-example-row" v-if="quiz&&response_code===0">
			<b-row>
				<b-col sm="6" offset-md="3">
					<QuestionBox
						v-if="display==true"
						v-bind:currentQuestion="questions[index]"
						v-bind:nextQuestion="nextQuestion"
						v-bind:increment="increment"
						v-bind:maximunQuestion="maximunQuestion"
						v-bind:totalCorrect="totalCorrect"
						v-bind:numCorrect="numCorrect"
						v-bind:quizTitle="quizTitle"
						>
					</QuestionBox>
				</b-col>
			</b-row>
		</b-container>
		<div>
			<b-modal id="modal-correctanswer">
        <div class="d-block">En hora Buena has escogido la Respuesta correcta</div>
      </b-modal>
      <b-modal id="modal-incorrectanswer">
        <div class="d-block">La respuesta es Incorrecta  la Respuesta correcta esta marcada en verde.</div>
      </b-modal>
		</div>
	</div>
</template>

<script>
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'
export default {
	name: 'app',
	components: {
		Header,
		QuestionBox,
		
	},

	data(){
		return{
			answers:[],
			questions:[],
			index:0,
			display:false,
			numCorrect:0,
      totalCorrect:0,
      questionary:true,
      maximunquestions:"0",
      response_code:3,
      quiz:false,
      quizTitle:0,
			formQuiz: {
				numberQuestions: ['1','2','3','4','5','6','7','8','9','10'],
				age: 0,
				selectedCategory:null,
				selectedDifficulty:null,
				selectedNumber:null,
				categories: [
					{ value:9, text:'General Knowledge' },
					{ value:10, text:'Books' },
					{ value:11, text:'Film' },
					{ value:12, text:'Music' },
					{ value:13, text:'Musicals and theatres' },
					{ value:14, text:'Television' },
					{ value:15, text:'Videogames' },
					{ value:16, text:'Board Games' },
					{ value:17, text:'Science and Nature' },
					{ value:18, text:'Computers' },
					{ value:19, text:'Mathematics' },
					{ value:20, text:'Mythology' },
					{ value:21, text:'Sports' },
					{ value:22, text:'Geography' },
					{ value:23, text:'History' },
					{ value:24, text:'Politics' },
					{ value:25, text:'Art' },
					{ value:26, text:'Celebrities' },
					{ value:27, text:'Animals' },
					{ value:28, text:'Vechicles' },
					{ value:29, text:'Comics' },
					{ value:30, text:'Gadgets' },
					{ value:31, text:'Anime and Manga' },
					{ value:32, text:'Cartoon Animation' }

				],
				difficulty: [
					{ value:'easy', text:'Easy' },
					{ value:'medium', text:'Medium' },
					{ value:'hard', text:'Hard' },
				]
			}

			
		}
	},
	methods:{
		nextQuestion:function(){
			this.index=this.index+1;
			console.log(this.index);
		},
		handleSubmit(){
			this.quiz = !this.quiz;
			this.questionary = !this.questionary;
			let quizTitle2= this.formQuiz.categories.find(category => {
				if (category.value==this.formQuiz.selectedCategory){
					return category.text; 
				}
			})
			this.quizTitle = quizTitle2.text;
			//Returning the Name of the category
			console.log(this.formQuiz.selectedDifficulty);
			console.log(this.formQuiz.selectedCategory);
            console.log(this.formQuiz.numberQuestions);
			let routetoquiz = 'https://opentdb.com/api.php?amount='+this.formQuiz.selectedNumber+'&category='+this.formQuiz.selectedCategory+'&difficulty='+this.formQuiz.selectedDifficulty+'&type=multiple'; 
			console.log(routetoquiz);
			fetch(routetoquiz, {
				method: 'get'
			})
				.then((response) => {
					return response.json();
				})
				.then ((jsonData) => {
					this.questions = jsonData.results;
					this.response_code= jsonData.response_code;
					this.display = true;
					this.maximunquestions = (this.questions.length);
				})
		},
		gettingCategories (){
			this.formQuiz.categories.map(category => {
				return category.text;
			})
		}
		,
		increment(isCorrect){
			if (isCorrect){
        this.numCorrect++;
        this.correctModal();
      }
      else {
        this.incorrectModal();
      }
			this.totalCorrect++;

		},
		maximunQuestion(){
			return this.maximunquestions; 

    },
    correctModal() {
      this.$root.$emit('bv::show::modal', 'modal-correctanswer', '#btnShow')
    },
    incorrectModal() {
      this.$root.$emit('bv::show::modal', 'modal-incorrectanswer', '#btnShow')
    }
  },
  
	mounted(){
	}
}
</script>

<style>
#app {
	font-family: 'Avenir', Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;
	color: #2c3e50;
	margin-top: 60px;
}
</style>
