<script>
    import { onMount } from 'svelte';

    let svg;
    let path;
/*
    onMount(() => {
        var distancePerPoint = 1;
        var drawFPS          = 60;

        
        var orig = path, length, timer;
        console.log(orig)

        function startDrawingPath(){
            length = 0;
            timer = setInterval(increaseLength,1000/drawFPS);
        }

        function increaseLength(){
            var pathLength = orig.getTotalLength();
            length += distancePerPoint;
            orig.style.strokeDasharray = [length,pathLength].join(' ');
            if (length >= pathLength) clearInterval(timer);
        }

        function stopDrawingPath(){
            clearInterval(timer);
            orig.style.stroke = '';
            orig.style.strokeDasharray = '';
        }
        //startDrawingPath()
    });*/


</script>

<div>

<svg bind:this={svg} viewBox="-0.15 0 100 100">

    <path bind:this={path} d="M3 0 L3 40"/>
    
</svg>

</div>


<style>

    

    div {
        width: 100%;
        height: 100vw;
        background-image: radial-gradient(circle at 2px 2px, rgba(0, 0, 0, 0.087) 2px, transparent 0);
        background-size: 1vw 1vw;
        position: relative;
    }
    @media screen and (max-width: 1000px) {
        div {
            background-size: 2vw 2vw;
        }
    }
    @media screen and (max-width: 600px) {
        div {
            background-size: 3vw 3vw;
        }
    }
    svg {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        fill: none;
        stroke: rgba(0, 0, 0, 0.211);
        stroke-width: 2px;
        
        
    }
    path {
       

        vector-effect: non-scaling-stroke;
    }


</style>