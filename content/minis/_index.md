+++
title = "minis"
+++

<div class="border px-5">

These are minis: posts that are too small or too random for me to call a blog post. 

</div>


<div class="border px-5 my-10">

It's named after the line of digital games available to download on PSPs from 2009 to 2015.


<div class="flex justify-center">
    <div id="tilt-card" class="relative group cursor-pointer" style="perspective: 1000px;">
        <div id="tilt-inner" class="relative transition-transform duration-200 ease-out" style="transform-style: preserve-3d;">
            <!-- Glow effect -->
            <div class="absolute -inset-4 bg-gradient-to-r from-purple-600 via-pink-500 to-cyan-400 rounded-xl opacity-0 group-hover:opacity-75 blur-xl transition-opacity duration-500" style="transform: translateZ(-20px);"></div>
            <!-- Card background -->
            <div class="absolute -inset-2 bg-gradient-to-br from-purple-500/20 via-pink-500/20 to-cyan-500/20 rounded-lg opacity-0 group-hover:opacity-100 transition-opacity duration-300" style="transform: translateZ(-10px);"></div>
            <!-- Image container -->
            <div class="relative p-2 rounded-lg bg-black/30 backdrop-blur-sm border border-white/10 group-hover:border-white/30 transition-all duration-300" style="transform: translateZ(20px);">
                <img src="minis.png" alt="A minis logo" width="100" height="100" class="rounded block transition-all duration-300 group-hover:scale-105">
                <!-- Shine effect - on same layer as image -->
                <div id="shine" class="absolute inset-0 rounded opacity-0 group-hover:opacity-100 transition-opacity duration-300 pointer-events-none" style="background: linear-gradient(105deg, rgba(255,255,255,0.25) 0%, rgba(255,255,255,0.15) 45%, transparent 50%);"></div>
            </div>
        </div>
    </div>
</div>

They're too small for [UMD disks](https://en.wikipedia.org/wiki/Universal_Media_Disc), but perfectly sized for digital download and storing at the time.

</div>



<script>
(function() {
    const card = document.getElementById('tilt-card');
    const inner = document.getElementById('tilt-inner');
    const shine = document.getElementById('shine');
    
    if (!card || !inner) return;
    
    let bounds;
    let isHovering = false;
    
    function rotateToMouse(e) {
        if (!isHovering) return;
        
        const mouseX = e.clientX;
        const mouseY = e.clientY;
        const leftX = mouseX - bounds.x;
        const topY = mouseY - bounds.y;
        const center = {
            x: leftX - bounds.width / 2,
            y: topY - bounds.height / 2
        };
        
        const rotateX = (-center.y / (bounds.height / 2)) * 25;
        const rotateY = (center.x / (bounds.width / 2)) * 25;
        
        inner.style.transform = `rotateX(${rotateX}deg) rotateY(${rotateY}deg)`;
        
        // Move shine based on mouse position - half diagonal shine
        if (shine) {
            const angle = 105 + (center.x / bounds.width) * 40;
            shine.style.background = `linear-gradient(${angle}deg, rgba(255,255,255,0.3) 0%, rgba(255,255,255,0.15) 45%, transparent 50%)`;
        }
    }
    
    card.addEventListener('mouseenter', function(e) {
        bounds = card.getBoundingClientRect();
        isHovering = true;
        document.addEventListener('mousemove', rotateToMouse);
    });
    
    card.addEventListener('mouseleave', function() {
        isHovering = false;
        document.removeEventListener('mousemove', rotateToMouse);
        inner.style.transform = 'rotateX(0deg) rotateY(0deg)';
    });
})();
</script>