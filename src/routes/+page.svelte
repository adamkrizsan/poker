<script lang="ts">
    import CommuntiyDeck from "../components/CommuntiyDeck.svelte";
    import PlayerSeat from "../components/PlayerSeat.svelte";

    //lookup table for card values 1-52

    let communityCards: number[] = [];
    const players: { id: number; hand: number[] }[] = [];

    function deal_cards() {
        const deck: number[] = Array.from({ length: 52 }, (_, i) => i); // Create a deck of 52 unique cards

        // Shuffle the deck
        for (let i = deck.length - 1; i > 0; i--) {
            const j = Math.floor(Math.random() * (i + 1));
            [deck[i], deck[j]] = [deck[j], deck[i]];
        }

        // Distribute cards to players
        for (let i = 0; i < 4; i++) {
            const hand = deck.splice(0, 2); // Take two unique cards from the deck
            players.push({ id: i, hand });
        }

        console.log(players);

        //distribution of community cards
        communityCards = deck.splice(0, 5);
    }
</script>

<main class="flex flex-col w-screen h-screen bg-gray-800">
    <div class="flex w-full justify-center">
        <h1
            class="flex text-6xl pt-10 text-gray-300 italic text-center font-serif"
        >
            Poker
        </h1>
    </div>
    <div class="flex py-10 justify-center">
        <button on:click={deal_cards}>asd</button>
        <CommuntiyDeck cards={communityCards} />
    </div>
    <div class="flex py-20 justify-center">
        {#each players as player}
            <PlayerSeat cards={player.hand} />
        {/each}
    </div>
</main>
