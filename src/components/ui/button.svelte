<script>
export let onClick
export let disabled
</script>

<button class="btn" on:click={onClick} disabled={disabled}><slot/></button>

<style>
.btn {
    padding: 0.5rem 1rem;
    background: transparent;
    border: #ff3e00 solid 1px;
    color: #ff3e00;
    cursor: pointer;
}

@media(max-width:300px) {
    .btn {
        padding: 0.1rem;
    }
}
</style>
