<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bestie Surprise</title>
    <style>
        body { 
            text-align: center; 
            font-family: "Helvetica", cursive, sans-serif; 
            background-color: lightpink; 
            overflow: hidden;
            position: relative;
        }
        .balloon { 
            width: 15vw; 
            height: 20vw; 
            border-radius: 50%; 
            position: absolute; 
            cursor: pointer; 
            display: flex; 
            align-items: center; 
            justify-content: center; 
            box-shadow: inset -5px -5px 10px rgba(0,0,0,0.2); 
        }
        .heart { 
            font-size: 50vw; 
            cursor: pointer; 
            display: none;
            transition: transform 0.3s ease-in-out;
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
        }
        .heart.pop { 
            transform: scale(1.5) translate(-50%, -50%);
            opacity: 0;
            transition: transform 0.3s ease-in-out, opacity 0.3s ease-in-out;
        }
        #message { 
            display: none; 
            font-size: 5vw; 
        }
        #page1 h2 { 
            position: relative; 
            z-index: 10; 
            font-size: 5vw;
        }
    </style>
</head>
<body>
    <div id="page1">
        <h2>Pop the Balloons!</h2>
        <div id="balloons"></div>
    </div>
    <div id="page2" style="display:none;">
        <h2>Click the Heart</h2>
        <span class="heart">❤️</span>
        <img id="pic" src="" style="display:none; width:50vw;">
    </div>
    <div id="page3" style="display:none;">
        <h2 id="message"></h2>
    </div>
    
    <script>
        let page = 1;
        let images = ['pic1.jpg', 'pic2.jpg', 'pic3.jpg', 'pic4.jpg', 'pic5.jpg'];
        let imgIndex = 0;
        let colors = ['red', 'blue', 'green', 'yellow', 'purple', 'orange', 'pink'];
        let positions = [];

        function isOverlapping(newPos) {
            for (let pos of positions) {
                let dx = newPos.x - pos.x;
                let dy = newPos.y - pos.y;
                let distance = Math.sqrt(dx * dx + dy * dy);
                if (distance < 18) return true;
            }
            return false;
        }

        // Generate Balloons
        for(let i = 0; i < 10; i++) {
            let balloon = document.createElement('div');
            balloon.classList.add('balloon');
            balloon.style.background = colors[Math.floor(Math.random() * colors.length)];
            
            let position;
            do {
                position = {
                    x: Math.random() * (window.innerWidth - 16),
                    y: Math.random() * (window.innerHeight - 22)
                };
            } while (isOverlapping(position));
            
            positions.push(position);
            balloon.style.left = position.x + 'px';
            balloon.style.top = position.y + 'px';
            
            balloon.onclick = function() {
                this.remove();
                new Audio('pop.mp3').play();
                if(document.querySelectorAll('.balloon').length === 0) {
                    document.getElementById('page1').style.display = 'none';
                    document.getElementById('page2').style.display = 'block';
                }
            }
            document.getElementById('balloons').appendChild(balloon);
        }
        
        // Heart Click Event
        document.querySelector('.heart').style.display = 'block';
        document.querySelector('.heart').onclick = function() {
            let heart = this;
            heart.classList.add('pop');
            setTimeout(() => {
                heart.style.display = 'none';
                if (imgIndex < images.length) {
                    document.getElementById('pic').src = images[imgIndex++];
                    document.getElementById('pic').style.display = 'block';
                    setTimeout(() => {
                        document.getElementById('pic').style.display = 'none';
                        heart.classList.remove('pop');
                        heart.style.display = 'block';
                    }, 5000);
                } else {
                    document.getElementById('page2').style.display = 'none';
                    document.getElementById('page3').style.display = 'block';
                    document.getElementById('message').innerText = "Your Special Note Here! Thank You!";
                }
            }, 300);
        }
    </script>
</body>
</html>
