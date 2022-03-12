<script>
import {
    array
} from '../../store/store'
import Button from '../ui/button.svelte'
export let sort, disabled, _i = undefined,
    _j = undefined

function* insertionsort() {
    let i, key, j
    for (i = 1; i < $array.length; i++) {
        key = $array[i], j
        for (j = i - 1; j >= 0 && $array[j] > key; j--) {
            _i = j + 1
            _j = j
            yield $array[j + 1] = $array[j]
        }
        yield $array[j + 1] = key
    }
}
</script>

<Button disabled={disabled} onClick={()=>sort(insertionsort)}>Insertion Sort</Button>
