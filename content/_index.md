---
title: "novel's site"
---


<div class="border-box" style="width: 20rem">
  <p>hi i am novel <span id="face">(つ b‿b つ)</span></p>
</div>

<div class="border-box">

* I am currently a video game programming student at Humber College
* I make [youtube videos](https://www.youtube.com/@mrabiry/videos)
* I make [music](https://www.youtube.com/watch?v=YFbU_HwXf7M&ab_channel=NovelAlex)

</div>

<div class="fc">
<div class="border-box">

you can find me on:


* [twitter](https://twitter.com/NovelAlexicus)
* [github](https://github.com/novelalex)

</div>

<div class="border-box fi2">

check out my [blog]({{<ref "blog">}}) while you're here <br> 
also here's a list of [quotes and articles]({{<ref "blog/list_of_really_good_quotes">}}) I like
</div>
</div>


<script>
  const faceElement = document.getElementById('face');
  const faces = ["(つ b‿b つ)", "(つ –‿– つ)"];
  let index = 0;

  setInterval(() => {
    index = 1 - index; // toggle between 0 and 1
    faceElement.textContent = faces[index];
  }, 2000);
</script>