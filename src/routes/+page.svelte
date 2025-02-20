<script lang="ts">
    import CommuntiyDeck from "../components/CommuntiyDeck.svelte";
    import PlayerSeat from "../components/PlayerSeat.svelte";

    //lookup table for card values 1-52

    let communityCards: number[] = $state([]);
    let players: { id: number; hand: number[] }[] = $state([]);
    let winner = $state<{ playerId: number; handName: string } | null>(null);

    function deal_cards() {
        players = [];
        communityCards = [];
        winner = null;
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
        

        //distribution of community cards
        communityCards = deck.splice(0, 5);
        //console.log("community cards" + communityCards);
        
        // Determine the winner
        const winningHand = determine_winner();
        winner = {
            playerId: winningHand.playerId,
            handName: winningHand.handName
        };
    }
    
    //function to calculate the winner
    function determine_winner() {
        const handRanks = [
            'Royal Flush',
            'Straight Flush',
            'Four of a Kind',
            'Full House',
            'Flush',
            'Straight',
            'Three of a Kind',
            'Two Pair',
            'One Pair',
            'High Card'
        ];

        function getCardRank(card: number): number {
            return card % 13;
        }

        function getCardSuit(card: number): number {
            return Math.floor(card / 13);
        }

        function evaluateHand(playerHand: number[], communityCards: number[]) {
            const allCards = [...playerHand, ...communityCards];
            const ranks = allCards.map(getCardRank).sort((a, b) => a - b);
            const suits = allCards.map(getCardSuit);

            // Count occurrences of each rank
            const rankCounts = new Map();
            ranks.forEach(rank => {
                rankCounts.set(rank, (rankCounts.get(rank) || 0) + 1);
            });

            // Count occurrences of each suit
            const suitCounts = new Map();
            suits.forEach(suit => {
                suitCounts.set(suit, (suitCounts.get(suit) || 0) + 1);
            });

            // Check for flush
            const hasFlush = Array.from(suitCounts.values()).some(count => count >= 5);

            // Check for straight
            let hasStraight = false;
            let straightHighCard = -1;
            for (let i = 0; i <= ranks.length - 5; i++) {
                if (
                    ranks[i + 4] - ranks[i] === 4 &&
                    new Set(ranks.slice(i, i + 5)).size === 5
                ) {
                    hasStraight = true;
                    straightHighCard = ranks[i + 4];
                    break;
                }
            }

            // Special case for Ace-low straight (A,2,3,4,5)
            if (!hasStraight && ranks.includes(12)) { // 12 is Ace
                const aceLowStraight = [0, 1, 2, 3, 12];
                if (aceLowStraight.every(rank => ranks.includes(rank))) {
                    hasStraight = true;
                    straightHighCard = 3; // 5 is the high card in A,2,3,4,5
                }
            }

            // Evaluate hand
            const pairs = Array.from(rankCounts.values()).filter(count => count === 2).length;
            const hasThreeOfAKind = Array.from(rankCounts.values()).some(count => count === 3);
            const hasFourOfAKind = Array.from(rankCounts.values()).some(count => count === 4);

            // Return hand strength and high card
            if (hasFlush && hasStraight && straightHighCard === 12) return { rank: 0, highCard: 12 }; // Royal Flush
            if (hasFlush && hasStraight) return { rank: 1, highCard: straightHighCard }; // Straight Flush
            if (hasFourOfAKind) return { rank: 2, highCard: Array.from(rankCounts.entries()).find(([_, count]) => count === 4)![0] }; // Four of a Kind
            if (hasThreeOfAKind && pairs > 0) return { rank: 3, highCard: Array.from(rankCounts.entries()).find(([_, count]) => count === 3)![0] }; // Full House
            if (hasFlush) return { rank: 4, highCard: Math.max(...ranks) }; // Flush
            if (hasStraight) return { rank: 5, highCard: straightHighCard }; // Straight
            if (hasThreeOfAKind) return { rank: 6, highCard: Array.from(rankCounts.entries()).find(([_, count]) => count === 3)![0] }; // Three of a Kind
            if (pairs === 2) return { rank: 7, highCard: Math.max(...Array.from(rankCounts.entries()).filter(([_, count]) => count === 2).map(([rank]) => rank)) }; // Two Pair
            if (pairs === 1) return { rank: 8, highCard: Array.from(rankCounts.entries()).find(([_, count]) => count === 2)![0] }; // One Pair
            return { rank: 9, highCard: Math.max(...ranks) }; // High Card
        }

        // Evaluate all players' hands
        const results = players.map(player => {
            const handResult = evaluateHand(player.hand, communityCards);
            return {
                playerId: player.id,
                handRank: handResult.rank,
                highCard: handResult.highCard,
                handName: handRanks[handResult.rank]
            };
        });

        // Sort by hand rank (lower is better) and high card
        results.sort((a, b) => {
            if (a.handRank !== b.handRank) {
                return a.handRank - b.handRank;
            }
            return b.highCard - a.highCard;
        });

        return results[0];
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
    
    <div class="flex pb-10 pt-30 justify-center">
        <button class="rounded-xl z-50 px-10" onclick={() => deal_cards()}><img src="/images/cardback.svg" alt="deck" class="h-45 border-4 border-gray-900 rounded-xl"/></button>
        <CommuntiyDeck cards={communityCards} />
    </div>
    <div class="flex py-20 justify-center">
        {#each players as player}
            <PlayerSeat 
                cards={player.hand}
                isWinner={winner?.playerId === player.id}
                handName={winner?.playerId === player.id ? winner.handName : ''}
            />
        {/each}
    </div>
</main>
