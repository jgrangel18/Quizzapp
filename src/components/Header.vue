<template>
    <div>
        <b-nav tabs>
            <b-nav-item active>Fancy Quiz</b-nav-item>
            <b-nav-item disabled>{{"Correct:"+numCorrect}}/{{"Total:"+totalCorrect}}</b-nav-item>
                    
         </b-nav>
    </div>
</template>
<script>
export default {
	props: [
		'numCorrect',
		'totalCorrect'
	]
	
}
</script>
