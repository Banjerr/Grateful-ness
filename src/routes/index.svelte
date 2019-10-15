<svelte:head>
	<title>Grateful-ness</title>
</svelte:head>

<script>
	import EntryCreator from '../components/EntryCreator.svelte';
	let quoteOfTheDay = '';
	function DayQuote(){
        var arrQuote = ["MYTH, AND CELEBRATION AND RELIGION IN OUR LIVES AND MUSIC IS A GOOD WAY TO ENCAPSULATE A LOT OF IT",
        "VIOLENCE IS NOT THE MONOPOLY OF THE EXPLOITERS AND AS SUCH THE EXPLOITED CAN USE IT TOO AND, MOREOVER, OUGHT TO USE IT WHEN THE MOMENT ARRIVES", 
        "LIVE LIFE EXPECTING THE WORST, HOPING FOR THE BEST, AND LIVING FOR THE FUTURE!",
        "NOTHING IS REALLY WORK UNLESS YOU WOULD RATHER BE DOING SOMETHING ELSE",
        "WITHOUT HUSTLE, TALENT WILL ONLY CARRY YOU SO FAR"];

        //var num = Math.floor(Math.random() * arrQuote.length);
        quoteOfTheDay = arrQuote[Math.floor(Math.random()*Math.floor(arrQuote.length))];
    }

    DayQuote()
</script>

<style>
	.form-quote{
        text-align: center;
        /*padding-top:50px;*/
        color:#777;
        font-size:28px;
    }
</style>

<!-- <figure> -->
	<!-- TODO make quote/image of the day components -->
	<!-- <img alt={imageOfTheDayAlt} src={imageOfTheDaySrc}> -->
	<!-- <figcaption>{{quoteOfTheDay}}</figcaption> -->
<!-- </figure> -->

<figure>
<figcaption class="form-quote">{quoteOfTheDay}</figcaption>
</figure>


<EntryCreator />
