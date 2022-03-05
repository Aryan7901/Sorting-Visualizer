
<script>
		import { flip } from 'svelte/animate';

export let length=30
$: speed=20
let min=5
let max=100
let timer
let _i,_j
let disabled=false
function swap(i,j){
	_i=i
	_j=j
    let temp=array[i]
    array[i]=array[j]
    array[j]=temp
}
function genArray(length) {
	clearInterval(timer)
	_i=_j=undefined
	disabled=false
  const array = [];
  for (let i = 0; i < length; i++) {
    array.push(Math.floor(Math.random() * (max - min + 1) + min));
  }
  return array;
}
function *quicksort(){
	duration=0
	function* split(start,end){
		if(start===end) return 
		let pivot=array[end]
		let i=start-1
		for(let j=start;j<end;j++){
			if(array[j]<pivot){
				i++
				yield swap(i,j)         
			}
		}
		yield swap(i+1,end)
			return i+1
	}
	function* sort(start,end){
		if(start<end){
			const j=yield* split(start,end)
			yield* sort(start,j-1)
			yield* sort(j+1,end)
		}
	
	}
	yield* sort(0,array.length-1)
}
function * bubblesort(){
	for(let i=0;i<array.length;i++){
		for(let j=i;j<array.length;j++){
			if(array[i]>array[j])
			yield swap(i,j)
		}
	}
}
function * mergesort(){

	function* merge(left,mid,right){
		let leftArray=array.slice(left,mid+1)
		let rightArray=array.slice(mid+1,right+1)
		let arr1_idx=0,arr2_idx=0,merged_idx=left

		while(arr1_idx<mid-left+1&& arr2_idx<right-mid){
			if(leftArray[arr1_idx]<=rightArray[arr2_idx]){
				_j=merged_idx
				yield array[merged_idx]=leftArray[arr1_idx]
				arr1_idx++
			}else{
				_j=merged_idx
				yield array[merged_idx]=rightArray[arr2_idx]
				arr2_idx++
			}
			merged_idx++
		}
		while(arr1_idx<mid-left+1){
			_j=merged_idx
			yield array[merged_idx]=leftArray[arr1_idx]
			arr1_idx++
			merged_idx++
		}
		while(arr2_idx<right-mid){
			_j=merged_idx
			yield array[merged_idx]=rightArray[arr2_idx]
			arr2_idx++
			merged_idx++
		}

	}

	function * sort(start,end){
		if(start>=end)return
		let mid=Math.floor(start+((end-start)/2))
		yield* sort(start,mid)
		yield* sort(mid+1,end)
		yield* merge(start,mid,end)
	}
		yield* sort(0,array.length-1)
}
function * selectionsort(){
	for(let i=0;i<array.length-1;i++){
		let min_idx=i
		for(let j=i+1;j<array.length;j++){
			if(array[j]<array[min_idx])
			yield min_idx=j
			
		}
		yield swap(min_idx,i)
	}
}


function * insertionsort(){
	let i,key,j
	for(i=1;i<array.length;i++){
		key=array[i],j
		for(j=i-1;j>=0&&array[j]>key;j--){
			_i=j+1
			_j=j
			yield array[j+1]=array[j]
			
		}
	yield array[j+1]=key


	}
}
function sort(sorter){
	disabled=true
	const gen=sorter()
 timer=setInterval(() => {
	const {done} =gen.next()
	if(done){
		clearInterval(timer)
		duration=800/speed
		_i=_j=undefined
		disabled=false
	}
}, 1000/speed);}
$:array=genArray(length)
function resetHandler(){
	duration=800/speed
	disabled=false
	array=genArray(length)
	clearInterval(timer)
	_i=_j=undefined
}
$: duration =800/speed
</script>

<main>
	<h1>Sorting Visualizer</h1>
	
	<div class="container">
		{#each array as bar ,index (index)}
		<div animate:flip="{{duration:duration}}" class="outer-container" style="width: {80/length}%;height: {bar}%;background:{index===_i||index===_j?"white":""} "/>
		{/each}
	</div>
	<button on:click={resetHandler} class="btn">Reset</button>
	<button disabled={disabled} on:click={()=>sort(bubblesort)} class="btn">Bubble Sort</button>
	<button disabled={disabled} on:click={()=>sort(selectionsort)} class="btn">Selection Sort</button>
	<button disabled={disabled} on:click={()=>sort(insertionsort)} class="btn">Insertion Sort</button>
	<button disabled={disabled} on:click={()=>sort(quicksort)} class="btn">Quick Sort</button>
	<button disabled={disabled} on:click={()=>sort(mergesort)} class="btn">Merge Sort</button>

	<button on:click={()=>{clearInterval(timer);disabled=false}} class="btn">Pause</button>
	<label for="size">Size</label>
	<input id="size" name="size" type="range" min="5" max="300" value={length} on:mouseleave={(event)=>length=event.target.value}/>
	<label for="speed">Speed</label>
	<input id="speed" name="speed" type="range" min="1" max="50" disabled={disabled} bind:value={speed}/>
</main>

<style>

	main {
		text-align: center;
		margin: 0 auto;
		display: block;

	}
	:global(body){
		background-color: white;
		margin: 0;
		
	}
	@media(prefers-color-scheme:dark){
		:global(body){
			background-color: black;
		}
	}
	.container{
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
	label{
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

.btn{
	padding:0.5rem 1rem;
	background: transparent;
	border: #ff3e00 solid 1px;
	color: #ff3e00;
	cursor: pointer;
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
  -webkit-appearance: none; /* Override default look */
  appearance: none;
  margin-top: -8px; /* Centers thumb on the track */
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
  border: none; /*Removes extra border that FF applies*/
  border-radius: 1rem;
  height: 1.5rem;
  width: 1.5rem;
}

input[type="range"]:focus::-moz-range-thumb{
  outline: 3px solid #ff3e00;
  outline-offset: 0.125rem;
}

	@media (max-width: 480px) {
		input[type="range"] {
			max-width: 80%;
		}
		h1{
			font-size: xx-large;
		}
	}
	@media(max-width:300px){
		.btn{
			padding: 0.1rem;
		}
	}
</style>