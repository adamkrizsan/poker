<script lang="ts">
    let props = $props();
    const cardValues = [
        "A",
        "2",
        "3",
        "4",
        "5",
        "6",
        "7",
        "8",
        "9",
        "10",
        "J",
        "Q",
        "K",
    ];
    let cardval = cardValues[props.cardval % 13];
</script>

<div class="flex p-1">
    <div class="w-25 h-35 rounded-xl transition-all bg-white">
        <h1 class="text-black p-2">{cardval}</h1>
    </div>
</div>
