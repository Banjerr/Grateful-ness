<script>
    let quoteOfTheDay = '';

	function DeadQuote(){
        var arrQuote = [
            "Talk about your plenty, talk about your ills, one man gathers what another man spills.",
            "Don't be a collector of more than you need, got a lot of things growing, but keep watching your seeds.", 
            "Some folks look for answers, others look for fights.",
            "Standing on the moon with nothing left to do. Lovely view of heaven, but I'd rather be with you.",
            "When I had no wings to fly, you flew to me.",
            "Even a blind man knows when the sun is shining.",
            "Inspiration, move me brightly.",
            "It was all night pouring, pouring rain, but not a drop on me.",
            "Without love in the dream it will never come true.",
            "If you get confused just listen to the music play."
        ];

        quoteOfTheDay = arrQuote[Math.floor(Math.random()*Math.floor(arrQuote.length))];
    }

    DeadQuote();
</script>

<style>
	figcaption {
        text-align: center;
        color: #777;
        text-transform: uppercase;
        font-size: 28px;
    }
</style>

<figure>
	<!-- TODO make quote/image of the day components -->
	<!-- <img alt={imageOfTheDayAlt} src={imageOfTheDaySrc}> -->
	<figcaption>{quoteOfTheDay}</figcaption>
</figure>