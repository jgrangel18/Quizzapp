<template>
		
    <div class="question-box-container">
		<link rel="stylesheet" href="../test.css">			
		<b-jumbotron>
			<template slot="header">Animals Quizz</template>

			<template slot="lead">
				{{currentQuestion.question}}
			</template>

			<hr class="my-4" >
			<b-list-group>
				<b-list-group-item id="qlist"  
				type="button"
				v-for="(question,index) in this.shuffledAnswers" 
				v-bind:key="index"
				v-on:click="selectedanswer(index)"
				v-bind:class="answeredclass(index)"
				>
					{{question}}
				</b-list-group-item>
			</b-list-group>
				
			<br>
			<b-button 
				v-on:click ="submitAnswer" 
				variant="primary"
				v-bind:disabled="answernum === null || answered === true " 
			>
				Submit

			</b-button>
			<b-button v-on:click="nextQuestion" variant="success"
				v-bind:disabled="answernum === null || answered === false || maximunQuestion()===totalCorrect"
			>
				Next
			</b-button>
			<div class="Puntaje">
				<h3> {{"Puntaje "+numCorrect+" / "+totalCorrect+":"+maximunQuestion()}}</h3>
			</div>
			
		</b-jumbotron>
		

	</div>
    
</template>
<script>
import _ from 'lodash'
export default {
	props:{
		currentQuestion:Object,
		nextQuestion:Function,
		increment:Function,
		maximunQuestion:Function,
		totalCorrect:Number,
		numCorrect:Number

	},
	data(){
		return{
			answernum:null,
			shuffledAnswers:[],
			correctAnswer:'',
			correct_index:null,
			answered:false,
			maxques:0
		}
	},
	methods:{
		selectedanswer: function(index){
			console.log("HOLA SOY EL INDEX"+index);
			this.answernum = index;
			console.log(this.answernum);
		},
		shuffleAnswers:function(){
			this.correct_index = null;
			let answers = [...this.currentQuestion.incorrect_answers,this.currentQuestion.correct_answer];
			this.correctAnswer = this.currentQuestion.correct_answer;
			this.shuffledAnswers = _.shuffle(answers);
			console.log("LA RESPUESTA CORRECTA ES"+this.correctAnswer);
			this.correct_index = this.shuffledAnswers.findIndex((item) => item == this.correctAnswer)}
			,
		submitAnswer:function(){
			console.log("ENTRE A LA FUNCION");
			console.log("correct index" + this.correct_index);
			console.log("ESCOGIDO" + this.answernum);
			let isCorrect = false;


			if (this.correct_index == this.answernum){
				isCorrect= true;

			}
			this.increment(isCorrect);
			this.answered = true;
			this.maxques= this.maximunQuestion;
			
			

 
		},

		
		answeredclass:function(index){
			let classanswer = '';
			if (this.answered === true && this.correct_index === index){
				classanswer = 'correct'
			}
			else if (this.answered ===true && this.answernum === this.correct_index && this.answernum ===index ){
				classanswer = 'correct'
			}
			else if (this.answered === false && this.answernum === index){
				classanswer = 'activeanswer'
			}
			else if (this.answered === true && this.answernum === index){
				classanswer = 'activeanswer'
			}
			return classanswer;
			
		}
				
	},	
	watch:{
		currentQuestion(){
			this.answernum = null;
			console.log("HOLLA LLAMANDO FUNCION SHUFFLE FELIZ");
			this.shuffleAnswers();
			this.answered = false;
		},
		//SPECIAL WAY TO PERFORM A WATCH METHOD TO EXECUTE WITHOUR UPDATE
		// currentQuestion2:{
		// 	inmediate:true,
		// 	handler(){
		// 		this.answernum=null;
		// 		console.log("HOLLA LLAMANDO FUNCION SHUFFLE FELIZ");
		// 		this.shuffleAnswers();

		// 	}

		// }


	},
	// ,
	// computed:{
	// 	completequestion:function(){
	// 		console.log("HOLA ENTRE AL LOOP");
	// 		let completequestion =[...this.currentQuestion.incorrect_answers];
	// 		completequestion.push(this.currentQuestion.correct_answer);
	// 		console.log (completequestion);
	// 		return completequestion;
	// 	}
	// }
	mounted (){
		this.shuffleAnswers()


	}
}

</script>


<style scoped>
	.list-group-item{
		background-color: rgba(244, 244, 245, 0.938);
	}
	.list-group-item:hover{
		background-color: rgba(161, 193, 224, 0.5);
	}
	.activeanswer{
		background-color: rgba(204, 70, 18, 0.5);

	}
	.correct{
		background-color: rgba(21, 80, 25, 0.5);

	}
	.Puntaje{
		margin-top: 10px;
	}


</style>
