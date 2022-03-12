<script>
import {
    array
} from '../../store/store'
import Button from '../ui/button.svelte'
export let sort
export let disabled
export let swap

function* bubblesort() {
    for (let i = 0; i < $array.length; i++) {
        for (let j = i; j < $array.length; j++) {
            if ($array[i] > $array[j])
                yield swap(i, j)
        }
    }
}
</script>

<Button disabled={disabled} onClick={()=>sort(bubblesort)}>Bubble Sort</Button>
