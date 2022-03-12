<script>
import {
    array
} from '../../store/store'
import Button from '../ui/button.svelte'
export let sort, disabled, swap, duration

function* quicksort() {
    duration = 0

    function* split(start, end) {
        if (start === end) return
        let pivot = $array[end]
        let i = start - 1
        for (let j = start; j < end; j++) {
            if ($array[j] < pivot) {
                i++
                yield swap(i, j)
            }
        }
        yield swap(i + 1, end)
        return i + 1
    }

    function* sort(start, end) {
        if (start < end) {
            const j = yield* split(start, end)
            yield* sort(start, j - 1)
            yield* sort(j + 1, end)
        }

    }
    yield* sort(0, $array.length - 1)
}
</script>

<Button disabled={disabled} onClick={()=>sort(quicksort)}>Quick Sort</Button>
