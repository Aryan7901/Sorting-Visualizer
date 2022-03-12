<script>
import {
    array
} from '../../store/store'
import Button from '../ui/button.svelte'
export let sort
export let disabled
export let swap

function* selectionsort() {
    for (let i = 0; i < $array.length - 1; i++) {
        let min_idx = i
        for (let j = i + 1; j < $array.length; j++) {
            if ($array[j] < $array[min_idx])
                yield min_idx = j

        }
        yield swap(min_idx, i)
    }
}
</script>

<Button disabled={disabled} onClick={()=>sort(selectionsort)}>Selection Sort</Button>
