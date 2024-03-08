---
toc: true
comments: false
layout: post
title: sprite animation 2
description: ummm sprite!!!
categories: [C7.0]
courses: { csse: {week: 0}, csp: {week: 2, categories: [2.C]}, csa: {week: 2} }
type: ccc
---

<body>
    <div>
        <canvas id="spriteContainer"> <!-- Within the base div is a canvas. An HTML canvas is used only for graphics. It allows the user to access some basic functions related to the image created on the canvas (including animation) -->
            <img id="fox_sprite" src="{{site.baseurl}}/images/fox_sprite.png">  // change sprite here
        </canvas>
        <div id="controls"> <!--basic radio buttons which can be used to check whether each individual animaiton works -->
            <input type="radio" name="animation" id="idle" checked>
            <label for="idle">Idle</label><br>
            <input type="radio" name="animation" id="idle_look">
            <label for="idle_look">Idle 2</label><br>
            <input type="radio" name="animation" id="walking">
            <label for="walking">Walking</label><br>
        </div>
    </div>
</body>

<script>
    // start on page load
    window.addEventListener('load', function () {
        const canvas = document.getElementById('spriteContainer');
        const ctx = canvas.getContext('2d');
        const SPRITE_WIDTH = 32;  // matches sprite pixel width
        const SPRITE_HEIGHT = 32; // matches sprite pixel height
        const FRAME_LIMIT = {
            'idle': 5,
            'idle_look': 14,
            'walking': 8,
            'jump':11,
            'scare':5,
            'sleep':6,
            'down':7,
        }

        const SCALE_FACTOR = 5;  // control size of sprite on canvas
        canvas.width = SPRITE_WIDTH * SCALE_FACTOR;
        canvas.height = SPRITE_HEIGHT * SCALE_FACTOR;

        class Fox {
            constructor() {
                this.image = document.getElementById("fox_sprite");
                this.x = 0;
                this.y = 0;
                this.minFrame = 0;
                this.maxFrame = FRAME_LIMIT['idle'];
                this.frameX = 0;
                this.frameY = 0;
            }

            // draw fox object
            draw(context) {
                context.drawImage(
                    this.image,
                    this.frameX * SPRITE_WIDTH,
                    this.frameY * SPRITE_HEIGHT,
                    SPRITE_WIDTH,
                    SPRITE_HEIGHT,
                    this.x,
                    this.y,
                    canvas.width,
                    canvas.height
                );
            }

            // update frameX of object
            update() {
                if (this.frameX < this.maxFrame) {
                    this.frameX++;
                } else {
                    this.frameX = 0;
                }
            }
        }

        // fox object
        const fox = new Fox();

        // update frameY of Fox object, action from idle, bark, walk radio control
        const controls = document.getElementById('controls');
        controls.addEventListener('click', function (event) {
            if (event.target.tagName === 'INPUT') {
                const selectedAnimation = event.target.id;
                switch (selectedAnimation) {
                    case 'idle':
                        fox.frameY = 0;
                        fox.maxFrame = FRAME_LIMIT['idle'];
                        break;
                    case 'idle_look':
                        fox.frameY = 1;
                        fox.maxFrame = FRAME_LIMIT['idle_look'];
                        break;
                    case 'walking':
                        fox.frameY = 2;
                        fox.maxFrame = FRAME_LIMIT['walking'];
                        break;
                    default:
                        break;
                }
            }
        });
        document.addEventListener('keydown', function(event) {
    switch (event.key) {
        case 'ArrowUp':
            fox.frameY = 3; // Assuming frameY index for upward movement animation
            fox.maxFrame = FRAME_LIMIT['jump'];
            break;
        case 'ArrowDown':
            fox.frameY = 6; // Assuming frameY index for downward movement animation
            fox.maxFrame = FRAME_LIMIT['down'];
            break;
        case 'ArrowLeft':
            fox.frameY = 4; // Assuming frameY index for leftward movement animation
            fox.maxFrame = FRAME_LIMIT['scare'];
            break;
        case 'ArrowRight':
            fox.frameY = 5; // Assuming frameY index for rightward movement animation
            fox.maxFrame = FRAME_LIMIT['sleep'];
            break;
        default:
            break;
    }

    // Start the animation loop
    animate();
});
        let animationTimeout;
        // Animation recursive control function
        function animate() {
            clearTimeout(animationTimeout);

            // Clears the canvas to remove the previous frame.
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            // Draws the current frame of the sprite.
            fox.draw(ctx);

            // Updates the `frameX` property to prepare for the next frame in the sprite sheet.
            fox.update();

           // Introduce a delay of 100 milliseconds (adjust as needed for desired speed)
            animationTimeout = setTimeout(function() {
                requestAnimationFrame(animate);
            }, 150); // Adjust delay time for slower or faster animation
        }

        // run 1st animate
        animate();
    });
</script>