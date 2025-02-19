<script lang="ts">
    import Card from "./Card.svelte";
    let props = $props();
    let cards: number[] = props.cards;
</script>

<div class="flex px-10">
    {#each cards as card}
        <Card cardval={card} />
    {/each}
</div>
