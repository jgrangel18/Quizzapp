<template>
	<div id="app">
		<Header
		v-bind:numCorrect="numCorrect"
		v-bind:totalCorrect="totalCorrect"
		/>
		<b-container class="bv-example-row">
			<b-row>
				<b-col sm="6" offset="3">
					<QuestionBox
						v-if="display==true"
						v-bind:currentQuestion="questions[index]"
						v-bind:nextQuestion="nextQuestion"
						v-bind:increment="increment"
						v-bind:maximunQuestion="maximunQuestion"
						v-bind:totalCorrect="totalCorrect"
						v-bind:numCorrect="numCorrect"
						>
						
						
					</QuestionBox>
				</b-col>
			</b-row>
		</b-container>
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
			maximunquestions:"0"
			
		}
	},
	methods:{
		nextQuestion:function(){
			console.log ("ENTRE A LA FUNCION")
			this.index=this.index+1;
			console.log(this.index);


		},
		increment(isCorrect){
			if (isCorrect){
				this.numCorrect++;
			}
			this.totalCorrect++;

		},
		maximunQuestion(){
			return this.maximunquestions; 

		}

	},
	mounted(){
		fetch('https://opentdb.com/api.php?amount=10&category=27&type=multiple', {
			method: 'get'
		})
			.then((response) => {
				return response.json();

			})
			.then ((jsonData) => {
				this.questions = jsonData.results; 
				console.log(this.questions);
				this.display = true;
				console.log("HOLA DISPLAY"+ this.display);
				this.maximunquestions = (this.questions.length);
				

			})
		
		
			
		
		
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
