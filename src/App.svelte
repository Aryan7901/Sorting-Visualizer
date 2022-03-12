<script>
import {
    flip
} from 'svelte/animate';
import {
    array
} from './store/store'
import Bubblesort from "./components/sorters/bubblesort.svelte";
import Selectionsort from "./components/sorters/selectionsort.svelte";
import Insertionsort from './components/sorters/insertionsort.svelte';
import Quicksort from './components/sorters/quicksort.svelte';
import Mergesort from './components/sorters/mergesort.svelte';
import Button from './components/ui/button.svelte'
export let length = 30
$: speed = 20
let min = 5
let max = 100
let timer
let _i, _j
let disabled = false

function swap(i, j) {
    _i = i
    _j = j
    let temp = $array[i]
    $array[i] = $array[j]
    $array[j] = temp
}

function genArray(length) {
    clearInterval(timer)
    _i = _j = undefined
    disabled = false
    const array = [];
    for (let i = 0; i < length; i++) {
        array.push(Math.floor(Math.random() * (max - min + 1) + min));
    }
    return array;
}

function sort(sorter) {
    disabled = true
    const gen = sorter()
    timer = setInterval(() => {
        const {
            done
        } = gen.next()
        if (done) {
            clearInterval(timer)
            duration = 2000 / Math.pow(speed, 2)
            _i = _j = undefined
            disabled = false
        }
    }, 1000 / speed);
}
$: ($array) = genArray(length)

function resetHandler() {
    duration = 2000 / Math.pow(speed, 2)
    disabled = false
    $array = genArray(length)
    clearInterval(timer)
    _i = _j = undefined
}
$: duration = 2000 / Math.pow(speed, 2)
</script>

<main>
    <h1>Sorting Visualizer</h1>

    <div class="container">
        {#each $array as bar ,index (index)}
        <div animate:flip="{{duration:duration}}" class="outer-container {index===_i||index===_j?"highlight":""}" style="width: {80/length}%;height: {bar}%; "/>
            {/each}
        </div>
        <Button onClick={resetHandler} class="btn">Reset</Button>
        <Bubblesort swap={swap} disabled={disabled} sort={sort}/>
        <Selectionsort swap={swap} disabled={disabled} sort={sort}/>
        <Insertionsort disabled={disabled} sort={sort} bind:_i={_i} bind:_j={_j}/>
        <Quicksort swap={swap} disabled={disabled} sort={sort} bind:duration={duration}/>
        <Mergesort disabled={disabled} sort={sort} bind:_i={_i} bind:_j={_j}/>
        <Button onClick={()=>{clearInterval(timer);disabled=false}}>Pause</Button>
        <label for="size">Size</label>
        <input id="size" name="size" type="range" min="5" max="300" value={length}  on:pointerleave={(event)=>length=event.target.value}/>
        <label for="speed">Speed</label>
        <input id="speed" name="speed" type="range" min="1" max="50" disabled={disabled} bind:value={speed}/>
</main>

<style>
main {
    text-align: center;
    margin: 0 auto;
    display: block;

}

:global(body) {
    background-color: white;
    margin: 0;

}

.container {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: flex-end;
    border: 0.05rem solid #ff3e00;
    height: 25rem;
    border-radius: 12px;
    margin: 5rem auto;
    width: 99%;
    overflow: hidden;
    margin: 2rem auto;
}

h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 4em;
    font-weight: 100;
}

label {
    color: #ff3e00;
    font-size: large;
}

.outer-container {
    height: 100%;

    display: flex;
    flex-direction: column;
    background-color: #ff3e00;
    min-height: 1;
}

.highlight {
    background: greenyellow;
}

@media(prefers-color-scheme:dark) {
    :global(body) {
        background-color: black;
    }

    .highlight {
        background: white;
    }

}

/*********** Baseline, reset styles ***********/
input[type="range"] {
    -webkit-appearance: none;
    appearance: none;
    background: transparent;
    cursor: pointer;
    width: 20rem;
    border: none;
}

/* Removes default focus */
input[type="range"]:focus {
    outline: none;
}

/******** Chrome, Safari, Opera and Edge Chromium styles ********/
/* slider track */
input[type="range"]::-webkit-slider-runnable-track {
    background-color: #e6c9ad;
    border-radius: 0.5rem;
    height: 0.5rem;
}

/* slider thumb */
input[type="range"]::-webkit-slider-thumb {
    -webkit-appearance: none;
    /* Override default look */
    appearance: none;
    margin-top: -8px;
    /* Centers thumb on the track */
    background-color: #ff3e00;
    border-radius: 1rem;
    height: 1.5rem;
    width: 1.5rem;
}

input[type="range"]:focus::-webkit-slider-thumb {
    outline: 3px solid #ff3e00;
    outline-offset: 0.125rem;
}

/*********** Firefox styles ***********/
/* slider track */
input[type="range"]::-moz-range-track {
    background-color: #e6c9ad;
    border-radius: 0.5rem;
    height: 0.5rem;
}

/* slider thumb */
input[type="range"]::-moz-range-thumb {
    background-color: #ff3e00;
    border: none;
    /*Removes extra border that FF applies*/
    border-radius: 1rem;
    height: 1.5rem;
    width: 1.5rem;
}

input[type="range"]:focus::-moz-range-thumb {
    outline: 3px solid #ff3e00;
    outline-offset: 0.125rem;
}

@media (max-width: 480px) {
    input[type="range"] {
        max-width: 80%;
    }

    h1 {
        font-size: xx-large;
    }
}
</style>
