---
title: "novel's site"
---

<div class="fc">
<div class="border-box" style="width: 16rem">
  <p>Hi, I am Novel <span id="face">(つ b‿b つ)</span></p>
</div>

<div class="border-box fi2">

I am a video game programmer.

</div>

</div>

<div class="fc">
<div class="border-box">

You can find me on:

* [Twitter](https://twitter.com/NovelAlexicus)
* [Github](https://github.com/novelalex)
* [Linkdin](https://www.linkedin.com/in/novel-alex/)

</div>

<div class="border-box fi2">

Check out my [blog]({{<ref "blog">}}) while you're here. <br> 
Or look at some of my [projects]({{<ref "projects">}}). <br> 
Also here's a list of [quotes, books, and articles]({{<ref "other/list_of_really_good_quotes">}}) I like.
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