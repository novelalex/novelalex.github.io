---
title: "novel's site"
---

<div class="fc">
<div class="border-box" style="width: 16rem">
  <p>hi i am novel <span id="face">(つ b‿b つ)</span></p>
</div>

<div class="border-box fi2">

i am a video game programmer.

</div>

</div>

<div class="fc">
<div class="border-box">

you can find me on:

* [twitter](https://twitter.com/NovelAlexicus)
* [github](https://github.com/novelalex)
* [linkdin](https://www.linkedin.com/in/novel-alex/)

</div>

<div class="border-box fi2">

check out my [blog]({{<ref "blog">}}) while you're here <br> 
or look at some of my [projects]({{<ref "projects">}}) <br> 
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