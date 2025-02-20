<script lang="ts">
    import Card from "./Card.svelte";
    let { cards } = $props();
</script>

<div class="flex">
    {#each cards as card}
        <Card cardval={card} />
    {/each}
</div>
