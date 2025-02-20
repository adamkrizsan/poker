<script lang="ts">
    import Card from "./Card.svelte";
    let { cards, isWinner = false, handName = '' } = $props();
</script>

<style>
    @keyframes wiggle {
        0% { transform: rotate(0deg); }
        25% { transform: rotate(1deg); }
        75% { transform: rotate(-1deg); }
        100% { transform: rotate(0deg); }
    }

    .winner-animation {
        animation: wiggle 0.2s ease-in-out 3;
    }

    .winner-background {
        background-color: rgb(34 197 94); /* This is the Tailwind green-500 color */
        border-radius: 0.75rem;
        padding: 0.5rem;
    }
</style>

<div class="flex flex-col items-center gap-2 transition-all duration-300" 
    class:winner-background={isWinner}
    class:winner-animation={isWinner}>
    <div class="flex px-10">
        {#each cards as card}
            <Card cardval={card} />
        {/each}
    </div>
    {#if isWinner && handName}
        <div class="text-green-900 font-bold text-xl">{handName}</div>
    {/if}
</div>
