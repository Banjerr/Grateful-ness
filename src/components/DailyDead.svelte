<script>
  let quoteOfTheDay,
    imageOfTheDaySrc,
    numberOfImages = 12;

  function DeadQuote() {
    var arrQuote = [
      "Talk about your plenty, talk about your ills, one man gathers what another man spills.",
      "Don't be a collector of more than you need, got a lot of things growing, but keep watching your seeds.",
      "Some folks look for answers, others look for fights.",
      "Standing on the moon with nothing left to do. Lovely view of heaven, but I'd rather be with you.",
      "When I had no wings to fly, you flew to me.",
      "Even a blind man knows when the sun is shining.",
      "Inspiration, move me brightly. Light the song with sense and color holds away despair",
      "It was all night pouring, pouring rain, but not a drop on me.",
      "Without love in the dream it will never come true.",
      "If you get confused just listen to the music play.",
      "You got to listen to the heavens, you got to try to understand. The greatness of their movement is just as small as it is grand.",
      "Ain't no time to hate, barely time to wait.",
      "Even a blind man knows when the sun is shining.",
      "When God way up in heaven, for whatever it was worth, thought he'd have a big old party, thought he'd call it planet Earth.",
      "Picture a bright blue ball just spinning, spinning free dizzy with eternity. Paint it with a skin of sky, brush in some clouds and sea call it home for you and me",
      "There is a road, no simple highway, between the dawn and the dark of night. And if you go no one may follow, that path is for your steps alone.",
      "Goin’ home, goin’ home. By the waterside I will rest my bones. Listen to the river sing sweet songs to rock my soul."
    ];

    quoteOfTheDay =
      arrQuote[Math.floor(Math.random() * Math.floor(arrQuote.length))];
  }

  function getRandomInt(min, max) {
    min = Math.ceil(min);
    max = Math.floor(max);
    return Math.floor(Math.random() * (max - min + 1)) + min;
  }

  function DeadImage() {
    const min = 1,
      max = numberOfImages;
    const randomImgNum = getRandomInt(min, max);

    imageOfTheDaySrc = randomImgNum;
  }

  DeadImage();
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
  <img alt="Grateful Dead" src="img/picture{imageOfTheDaySrc}.jpg">
  <figcaption>{quoteOfTheDay}</figcaption>
</figure>
