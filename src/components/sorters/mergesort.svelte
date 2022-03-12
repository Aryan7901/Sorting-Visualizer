<script>
import {
    array
} from '../../store/store'
import Button from '../ui/button.svelte'
export let sort, disabled, _i = undefined,
    _j = undefined

function* mergesort() {
    _i = undefined

    function* merge(left, mid, right) {
        let leftArray = $array.slice(left, mid + 1)
        let rightArray = $array.slice(mid + 1, right + 1)
        let arr1_idx = 0,
            arr2_idx = 0,
            merged_idx = left

        while (arr1_idx < mid - left + 1 && arr2_idx < right - mid) {
            if (leftArray[arr1_idx] <= rightArray[arr2_idx]) {
                _j = merged_idx
                yield $array[merged_idx] = leftArray[arr1_idx]
                arr1_idx++
            } else {
                _j = merged_idx
                yield $array[merged_idx] = rightArray[arr2_idx]
                arr2_idx++
            }
            merged_idx++
        }
        while (arr1_idx < mid - left + 1) {
            _j = merged_idx
            yield $array[merged_idx] = leftArray[arr1_idx]
            arr1_idx++
            merged_idx++
        }
        while (arr2_idx < right - mid) {
            _j = merged_idx
            yield $array[merged_idx] = rightArray[arr2_idx]
            arr2_idx++
            merged_idx++
        }

    }

    function* sort(start, end) {
        if (start >= end) return
        let mid = Math.floor(start + ((end - start) / 2))
        yield* sort(start, mid)
        yield* sort(mid + 1, end)
        yield* merge(start, mid, end)
    }
    yield* sort(0, $array.length - 1)
}
</script>

<Button disabled={disabled} onClick={()=>sort(mergesort)}>Merge Sort</Button>
