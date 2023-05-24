<script>
	import { onMount } from 'svelte';

	let canvas = null;

    const duration = 10000;

    const paths = [
        
        {   // 0
            path: "M 4 2 L 8 2 C 9 2 10 3 10 4 C 10 5 9 6 8 6 L 4 6 C 3 6 2 5 2 4 C 2 3 3 2 4 2",
            position: [1, 1],
            enter: [.05, .1],
            exit: [.3, .4]
        },
        // phone
        {  
            path: "M 2 15 L 6 15 C 8 15 8 15 8 13 L 8 2 C 8 0 8 0 6 0 L 2 0 C 0 0 0 0 0 2 L 0 13 C 0 15 0 15 2 15",
            position: [80, 1],
            enter: [.2, .4],
            exit: [.5, .6],
            id: 'phone0'
        },
        {
            path: "M 3 1 L 1 1",
            position: [2, -0.5],
            enter: [.2, .4],
            exit: [.5, .6],
            parent: 'phone0'
        },
        // sections
        {
            path: "M 8 1 L 1 1",
            position: [-0.5, 2],
            enter: [.2, .4],
            exit: [.5, .6],
            parent: 'phone0'
        },
        {
            path: "M 8 1 L 1 1",
            position: [-0.5, 4],
            enter: [.2, .4],
            exit: [.5, .6],
            parent: 'phone0'
        },
        {
            path: "M 8 1 L 1 1",
            position: [-0.5, 6],
            enter: [.2, .4],
            exit: [.5, .6],
            parent: 'phone0'
        },

        {
            path: "M 1 0 L 22 0 C 23 0 23 0 23 1 L 23 12 C 23 13 23 13 22 13 L 1 13 C 0 13 0 13 0 12 L 0 1 C 0 0 0 0 1 0",
            position: [50, 1],
            enter: [.2, .4],
            exit: [.5, .6],
            id: 'desktop0'
        },
        {
            path: "M 23 0 L 0 0",
            position: [0, 1],
            enter: [.2, .4],
            exit: [.5, .6],
            id: 'desktop1-navbar',
            parent: 'desktop0'
        },
        {
            path: "M 4 0 L 4 12",
            position: [0, 1],
            enter: [.2, .4],
            exit: [.5, .6],
            id: 'desktop1-sidebar',
            parent: 'desktop0'
        },
        {
            path: "M 2 0 L 0 0",
            position: [0.5, 1],
            enter: [.2, .4],
            exit: [.5, .6],
            id: 'desktop1-sidebar-text1',
            parent: 'desktop1-sidebar'
        },
        {
            path: "M 2 0 L 0 0",
            position: [0.5, 2],
            enter: [.2, .4],
            exit: [.5, .6],
            id: 'desktop1-sidebar-text2',
            parent: 'desktop1-sidebar'
        },
        {
            path: "M 2 0 L 0 0",
            position: [0.5, 3],
            enter: [.2, .4],
            exit: [.5, .6],
            id: 'desktop1-sidebar-text3',
            parent: 'desktop1-sidebar'
        },

        {
            path: "M 1 0 L 22 0 C 23 0 23 0 23 1 L 23 12 C 23 13 23 13 22 13 L 1 13 C 0 13 0 13 0 12 L 0 1 C 0 0 0 0 1 0",
            position: [50, 22],
            enter: [.2, .4],
            exit: [.5, .6],
            id: 'desktop1'
        },
        {
            path: "M 23 0 L 0 0",
            position: [0, 1],
            enter: [.2, .4],
            exit: [.5, .6],
            id: 'desktop1-navbar',
            parent: 'desktop1'
        },
        {
            path: "M 1 0 L 9 0 C 10 0 10 0 10 1 L 10 9 C 10 10 10 10 9 10 L 1 10 C 0 10 0 10 0 9 L 0 1 C 0 0 0 0 1 0",
            position: [0, 1],
            enter: [.2, .4],
            exit: [.5, .6],
            id: 'desktop1-navbar',
            parent: 'desktop1',
            scale: 0.5,
        },
        
    ];

    const ellipses = [
        {
            radius: 1.3,
            position: [5, 5],
            enter: [.1, .2],
            exit: [.3, .4]
        },
        {
            radius: 0.5,
            position: [1, 2],
            enter: [.1, .2],
            exit: [.3, .4],
            parent: 'phone0'
        },
        {
            radius: 0.5,
            position: [1, 4],
            enter: [.1, .2],
            exit: [.3, .4],
            parent: 'phone0'
        },
        {
            radius: 0.5,
            position: [1, 6],
            enter: [.1, .2],
            exit: [.3, .4],
            parent: 'phone0'
        }
    ]

    const pathsBindings = [];

    const testing = {
        
        enabled: true,
        static: true,
        paths: [1, 2],
        all: true,
        
    }

    //const path = new Path2D("M30 10 L30 100 L60 100");

    function draw() {
		const ctx = canvas.getContext('2d');

        const dotSize = 2;
        let dotSpacing = window.innerWidth < 1000 ? window.innerWidth < 600? 10 : 12 : 20;

        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;

        const numDotsX = Math.floor(canvas.width / dotSpacing);
        const numDotsY = Math.floor(canvas.height / dotSpacing);

        scalePaths(dotSpacing)

        drawGrid();

        if (testing.enabled) drawTest();
        else requestAnimationFrame(drawPaths);

        
        

        

        let startTime = 0;

        function drawGrid () {
            for (let x = 0; x < numDotsX; x++) {
            for (let y = 0; y < numDotsY; y++) {
                ctx.fillStyle = 'rgba(0,0,0,0.2)'; // Set dot color

                // Calculate the dot position
                const dotX = x * dotSpacing + dotSpacing / 2;
                const dotY = y * dotSpacing + dotSpacing / 2;



                // Draw the dot
                ctx.fillRect(dotX, dotY, dotSize, dotSize);
                }
            }
        }


        function drawPaths (timestamp) {
            
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            
            if (!startTime) startTime = timestamp;
            const elapsed = timestamp - startTime;
            const frame = (elapsed % duration) / duration;
            
            //for (let i = 0; i < paths.length; i++) {

            paths.map((path) => {
                const totalLength = path.pathLength;
                
                // between
                if (frame >= path.enter[1] && frame <= path.exit[0]) {
                    currentLength = totalLength;
                } 
                // enter
                else if (frame >= path.enter[0] && frame <= path.enter[1]) {
                    currentLength = (frame - path.enter[0]) / (path.enter[1] - path.enter[0]) * totalLength;
                } 
                // exit
                else if (frame >= path.exit[0] && frame <= path.exit[1]) {
                    currentLength = totalLength - (frame - path.exit[0]) / (path.exit[1] - path.exit[0]) * totalLength;
                } 
                // before enter
                else if (frame >= path.exit[1]) {
                    currentLength = 0;
                } 
                //after exit
                else if (frame <= path.enter[0]) {
                    currentLength = 0;
                }

                const newPath = new Path2D(path.scaledPath);
                
                ctx.setLineDash([currentLength, totalLength]);
                ctx.stroke(newPath);

            })
            drawGrid();

            requestAnimationFrame(drawPaths)
        }
        
        function drawTest () {

            if (testing.static) {
                 
                paths.map((path, index) => {
                    if (testing.all) path.testing = true;
                    else if (testing.paths.includes(index)) path.testing = true;
                    else path.testing = false;
                })

                for (let i = 0; i < paths.length; i++) {
                    if (!paths[i].testing) continue;
                    
                    const path = new Path2D(paths[i].scaledPath);
                    ctx.stroke(path);
                }
                if (testing.all) {
                    for (let i = 0; i < ellipses.length; i++) {
                        console.log('ellipses')
                        ctx.beginPath();
                        ctx.ellipse(
                            ellipses[i].scaledPosition[0], ellipses[i].scaledPosition[1], 
                            ellipses[i].scaledRadius, ellipses[i].scaledRadius, 
                            0, 0, Math.PI * 2);
                            
                        ctx.stroke();
                    }
                }


                return;
            }

            const minFrame = Math.min(...testing.paths.map((path) => paths[path].enter[0]))
            const maxFrame = Math.max(...testing.paths.map((path) => paths[path].exit[1]))

            
            requestAnimationFrame(drawTestPaths)
            
            

            function drawTestPaths (timestamp) {
                ctx.clearRect(0, 0, canvas.width, canvas.height);
                
                if (!startTime) startTime = timestamp;
                const elapsed = timestamp - startTime;
                
                const frame = ((elapsed % duration) / duration) * (maxFrame - minFrame) + minFrame;

                for (let i = 0; i < testing.paths.length; i++) {

                    const path = paths[testing.paths[i]];
                    const enter = path.enter;
                    const exit = path.exit;
                    
                    const totalLength = path.pathLength;

                    const currentLength = animationStep(frame, enter, exit, totalLength);

                    const newPath = new Path2D(path.scaledPath);
                    
                    ctx.setLineDash([currentLength, totalLength]);
                    ctx.stroke(newPath);

                }
                
                for (let i = 0; i < ellipses.length; i++) {

                    const totalLength = ellipses[i].radius * dotSpacing * Math.PI * 2;
                    const currentLength = animationStep(frame, ellipses[i].enter, ellipses[i].exit, totalLength);
                    ctx.beginPath();
                    ctx.setLineDash([currentLength, totalLength]);
                    ctx.ellipse(
                        ellipses[i].scaledPosition[0], ellipses[i].scaledPosition[1], 
                        ellipses[i].scaledRadius, ellipses[i].scaledRadius, 
                        0, 0, Math.PI * 2);
                        
                    ctx.stroke();
                }


                drawGrid();

                requestAnimationFrame(drawTestPaths)
            };
        }
        
    }

    function animationStep (frame, enter, exit, totalLength) {
        // between
        if (frame >= enter[1] && frame <= exit[0]) {
            return totalLength;
        } 
        // enter
        if (frame >= enter[0] && frame <= enter[1]) {
            return (frame - enter[0]) / (enter[1] - enter[0]) * totalLength;
        } 
        // exit
        if (frame >= exit[0] && frame <= exit[1]) {
            return totalLength - (frame - exit[0]) / (exit[1] - exit[0]) * totalLength;
        } 
        // before enter
        if (frame >= exit[1]) {
            return 0;
        } 
        //after exit
        if (frame <= enter[0]) {
            return 0;
        }
    }

    function scalePaths (dotSpacing) {
        for (let i = 0; i < paths.length; i++) {

            if (paths[i].parent) {
                const parent = paths.find(path => path.id == paths[i].parent);
                paths[i].position = [
                    paths[i].position[0] + parent.position[0],
                    paths[i].position[1] + parent.position[1]
                ]
            }

            let matchCount = 0;
            paths[i].scaledPath = paths[i].path.replace(/\d+/g, (match, index) => {
                
                const isX = matchCount % 2 === 0;
                matchCount += 1;
                let offset = paths[i].position? paths[i].position[isX? 0:1] : 0;

                const scale = paths[i].scale || 1;

                return match * scale * dotSpacing + offset * dotSpacing + dotSpacing / 2 + 1;
            });
            paths[i].pathLength = pathsBindings[i].getTotalLength() * dotSpacing; 
        }

        for (let i = 0; i < ellipses.length; i++) {

            if (ellipses[i].parent) {
                
                const parent = paths.find(path => path.id == ellipses[i].parent);
                ellipses[i].position = [
                    ellipses[i].position[0] + parent.position[0],
                    ellipses[i].position[1] + parent.position[1]
                ]
            }
            
            ellipses[i].scaledRadius = ellipses[i].radius * dotSpacing;
            ellipses[i].pathLength = ellipses[i].scaledRadius * Math.PI * 2; 
            ellipses[i].scaledPosition = [
                ellipses[i].position[0] * dotSpacing + dotSpacing / 2 + 1, 
                ellipses[i].position[1] * dotSpacing + dotSpacing / 2 + 1
            ];
        }

    }

    function handleResize() {
        
        draw();
    }

	onMount(() => {
        //draw();
/*
        window.addEventListener('resize', drawCanvas);
        return () => {
			window.removeEventListener('resize', drawCanvas);
		};*/


		let frame = requestAnimationFrame(draw);


/*
		return () => {
			cancelAnimationFrame(draw);
		};*/
	});

</script>



<canvas bind:this={canvas}>

    <svg>

        {#each paths as path, i}
            <path d={path.path} bind:this={pathsBindings[i]} />
        {/each}

        <path 
        d="M 4 2 L 8 2 C 9 2 10 3 10 4 C 10 5 9 6 8 6 L 4 6 C 3 6 2 5 2 4 C 2 3 3 2 4 2" 
        enter={[0, .1]} exit={[.3, .4]} />

        <path 
        d="M50 10 L50 80 L90 100" 
        enter={[.2, .4]} exit={[.8, 1]} />
    </svg>
</canvas>

<style>
	canvas {
		width: 100%;
		height: 100%;
		background-color: #ffffff;
	}
    svg {
        display: none;
    }
</style>