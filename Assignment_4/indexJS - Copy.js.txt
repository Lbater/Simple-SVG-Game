//Author: Logan Bateman, 000918989

let node = document.getElementById('player');

//Start of buttons
moveLeft = () =>{
    let move = parseInt(node.getAttribute('x')) - 50;
    node.setAttribute('x', move);
}

moveRight = () =>{
    let move = parseInt(node.getAttribute('x')) + 50;
    node.setAttribute('x', move);
}

timer = () =>{
    let circlesInput = document.getElementById('circles');
    let circlesValue = parseInt(circlesInput.value);
    if(circlesValue > 0 || !isNaN(circlesInput)){
        setInterval(move(), 2500);
        setInterval(points, 100);
        circleSpawning();

        let inscructions = document.getElementById('instructions');
        inscructions.innerHTML = "Don't get hit or let the timer hit 0";
        
        let easterEgg = document.getElementById('secret');
        easterEgg.innerHTML = "Hover over the player for a suprise";

        let score = document.getElementById('score');
        score.innerHTML = "Score: ";

        //circles.parentNode.removeChild(circlesInput);
    }else{
        let inscructions = document.getElementById('empty');
        inscructions.innerHTML = "Please input a number of circles";
    }
}

refresh = () =>{
    window.location.reload();
}
//End of buttons

//Start of point counter
let point = 0;
points = () => {
    let circles = document.querySelectorAll('circle');
    let svgTop = 0;

    circles.forEach(circle => {
        let currentY = parseInt(circle.getAttribute('cy'));
        if (currentY <= svgTop) {
            circle.remove();
            point++;
            resetPB();
        }
        let score = document.getElementById('score');
        score.innerHTML = "Score: " + point;
    });
    if (document.querySelectorAll('circle').length === 0) {
        circleSpawning();
    }
}
//End of point counter

//Start circle spawning
circleSpawning = () =>{
    let svg = document.getElementById('playField');
    let circlesInput = document.getElementById('circles');
    let circlesValue = parseInt(circlesInput.value);

    clearInterval(moveCirclesUp);
    
    for(let i = 0; i < circlesValue; i++){
        let circle = document.createElementNS('http://www.w3.org/2000/svg', 'circle');
        let randomX = Math.floor(Math.random() * parseInt(svg.getAttribute('width')) * 5);
        circle.setAttribute('cx', randomX);
        circle.setAttribute('cy', '480');
        circle.setAttribute('r', '20');
        circle.setAttribute('fill', 'purple');
        circle.setAttribute('stroke', 'black');
        circle.setAttribute('stroke-width', '2');
        circle.setAttribute('id', 'circle');
        svg.appendChild(circle); // Append circle to the SVG element
    }
    setInterval(moveCirclesUp, 100);
}

moveCirclesUp = () => {
    let circles = document.querySelectorAll('circle');
    circles.forEach(circle => {
        let currentY = parseInt(circle.getAttribute('cy'));
        circle.setAttribute('cy', currentY - 20);
    });
}
//End circle spawning

//Start of progress bar
let i = 0;
let progress = 2500;
move = () =>{
	if (i === 0) {
		i = 1;
		let barElement = document.getElementById('myBar');
		let id = setInterval(frame, 10);
		function frame() {
			if (progress <= 0) {
				clearInterval(id);
				i = 0;
                refresh();
			} else {
                progress -= 2;
                let width = (progress / 2500) * 100;
                barElement.style.width = width + '%';
                barElement.innerHTML = Math.round(width) + '%';
			}
		}
	}
}
//Reset the progress bar
resetPB = () =>{
    progress = 2500;
    move();
}
//End of progress bar

//Start of collision detection
isColliding = (player, circle) =>{
    return !(  
            player.left > circle.right ||
            circle.left > player.right ||
            player.top > circle.bottom ||
            circle.top > player.bottom
        );
}

getBoundingBox = (element) =>{
    let rect = element.getBoundingClientRect();
    return {
        left: rect.left,
        right: rect.right,
        top: rect.top,
        bottom: rect.bottom
    };
}

checkCollision = () => {
    let player = getBoundingBox(document.getElementById('player'));
    let circles = document.querySelectorAll('circle');

    circles.forEach(circle => {
        let circleBoundingBox = getBoundingBox(circle);
        if (isColliding(player, circleBoundingBox)) {
            window.location.reload();
        }
    });
}

setInterval(checkCollision, 100);
//End of collision detection

//Start of changing the square color on mouse hover
node.addEventListener('mouseover', function() {
    node.setAttribute('fill', 'blue');
    node.setAttribute('width', '100');
    node.setAttribute('height', '100');
});

node.addEventListener('mouseout', function() {
    node.setAttribute('fill', 'red');
    node.setAttribute('width', '80');
    node.setAttribute('height', '80');
});
//End of changing the square color on mouse hover