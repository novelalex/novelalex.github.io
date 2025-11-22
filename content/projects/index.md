---
title: "projects"
---

#### I’m currently <span id="verb">building</span>

<div class="fc">
<div class="card">
  <img src="rent_dungeon/rent_dungeon_screenshot.png" alt="A Dark pixelated dungeon">

  <div class="overlay">
    <b>Rent Dungeon</b>
    <p>A first person dungeon crawler</p>
  </div>
</div>

<div class="card">
  <img src="gaussian_splatting/gaussian_splatting_screenshot.png" alt="A Pointcloud of a scene">

  <div class="overlay">
    <b>Gaussian Splatting Exploration</b>
    <p>Learning more about gaussian splatting by building a renderer based on the <a href="https://repo-sam.inria.fr/fungraph/3d-gaussian-splatting/3d_gaussian_splatting_low.pdf">paper</a></p>
  </div>
</div>
</div>
<br>

#### I have worked on



<div class="fc">

<div class="card">
  <img src="pyro_check/pyro_check_screenshot.png" alt="A Chessboard, some dice and a lighter">
  <div class="overlay">
    <b><a href="https://zoeeechu.itch.io/pyro-check">Pyro Check</a></b>
    <p>Chess with a twist. <br> <a style="font-size: 0.8rem;" href="https://torontogamesweek.com/schedule/2024.html#day7">[Showcased at Toronto Games Week 2024]</a></p>
  </div>
</div>

<div class="card">
  <img src="buh_buh_buh_bubbles/bubble_banner.png" alt="A Banner for Buh Buh Buh Bubbles">
  <div class="overlay">
    <b><a href="https://zoeeechu.itch.io/buhbuhbuhbubbles">Buh Buh Buh Bubbles</a></b>
    <p>Navigate a bubble out of the perilous depths</p>
    <p style="font-size: 0.8rem; margin: 0 0 0 0">Role: Musican</p>
  </div>
</div>

<div class="card">
  <img src="light_lock/LightLock_poster.png" alt="A Poster for LightLock">
  <div class="overlay">
    <b>LightLock</b>
    <p >Co-op puzzle platformer where everybody is chained together. <br> <a style="font-size: 0.8rem;" href="https://levelupshowcase.com/archive/">[Showcased at Level Up 2025]</a></p>
  </div>
</div>

</div>


<br>

<script>
  class TextScramble {
  constructor(el) {
    this.el = el;
    this.chars = "!<>-_\\/[]{}—=+*^?#________";
    this.update = this.update.bind(this);
  }

  setText(newText) {
    const oldText = this.el.innerText;
    const length = Math.max(oldText.length, newText.length);
    const promise = new Promise((resolve) => (this.resolve = resolve));
    this.queue = [];

    for (let i = 0; i < length; i++) {
      const from = oldText[i] || "";
      const to = newText[i] || "";
      const start = Math.floor(Math.random() * 40);
      const end = start + Math.floor(Math.random() * 40);
      this.queue.push({ from, to, start, end });
    }

    cancelAnimationFrame(this.frameRequest);
    this.frame = 0;
    this.update();
    return promise;
  }

  update() {
    let output = "";
    let complete = 0;

    for (let i = 0, n = this.queue.length; i < n; i++) {
      let { from, to, start, end, char } = this.queue[i];

      if (this.frame >= end) {
        complete++;
        output += to;
      } else if (this.frame >= start) {
        if (!char || Math.random() < 0.28) {
          char = this.randomChar();
          this.queue[i].char = char;
        }
        output += `<span class="dud">${char}</span>`;
      } else {
        output += from;
      }
    }

    this.el.innerHTML = output;

    if (complete === this.queue.length) {
      this.resolve();
    } else {
      this.frameRequest = requestAnimationFrame(this.update);
      this.frame++;
    }
  }

  randomChar() {
    return this.chars[Math.floor(Math.random() * this.chars.length)];
  }
}

// Phrases to rotate
const phrases = [
  "building",
  "tinkering on",
  "exploring",
  "hacking on",
  "messing with",
  "working on",
  "tinkering with"
];

const el = document.getElementById("verb");
const fx = new TextScramble(el);

let counter = 0;
function next() {
  fx.setText(phrases[counter]).then(() => {
    setTimeout(next, 3000);
  });
  counter = (counter + 1) % phrases.length;
}

next();
</script>