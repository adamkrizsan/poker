<script lang="ts">
    let { cardval } = $props();
    const cardValues = [
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
        "A"
    ];
    let card = $derived(cardValues[cardval % 13]);
    let suitSVG = 
         [`<svg width="32px" height="32px" viewBox="0 0 256 256" xmlns="http://www.w3.org/2000/svg" fill="#890505"><g id="SVGRepo_bgCarrier" stroke-width="0"></g><g id="SVGRepo_tracerCarrier" stroke-linecap="round" stroke-linejoin="round"></g><g id="SVGRepo_iconCarrier"><path fill="#000000" d="M229.4575 153.8525c0 31.315-27 45.66-45.67 45.67-20.82 0-36.55-9.43-45.915-17.13 1.235 25.475 7.265 43.675 34.325 58h-88.395c27.06-14.325 33.09-32.525 34.325-58-9.365 7.695-25.1 17.14-45.915 17.13-18.645 0-45.67-14.355-45.67-45.67 0-57.235 40.32-41.66 101.455-138.245 61.14 96.585 101.46 81.015 101.46 138.245z"></path></g></svg>`,
         `<svg fill="#890505" width="38px" height="38px" viewBox="4 4 30 30" version="1.1" xmlns="http://www.w3.org/2000/svg"><g id="SVGRepo_bgCarrier" stroke-width="0"></g><g id="SVGRepo_tracerCarrier" stroke-linecap="round" stroke-linejoin="round"></g><g id="SVGRepo_iconCarrier"> <title>hearts</title> <path d="M27.267 11.489c0.21 6.687-10.266 11.384-11.25 15.056-1.075-4.011-11.060-8.078-11.283-15.056-0.214-6.701 8.047-8.155 11.283-2.55 3.316-5.743 11.043-4.039 11.25 2.55z"></path> </g></svg>`,
         `<svg width="34px" height="34px" viewBox="40 40 450 450" xmlns="http://www.w3.org/2000/svg" fill="#890505"><g id="SVGRepo_bgCarrier" stroke-width="0"></g><g id="SVGRepo_tracerCarrier" stroke-linecap="round" stroke-linejoin="round"></g><g id="SVGRepo_iconCarrier"><path fill="#890505" d="M431.76 256c-69 42.24-137.27 126.89-175.76 224.78C217.51 382.89 149.25 298.24 80.24 256c69-42.24 137.27-126.89 175.76-224.78C294.49 129.11 362.75 213.76 431.76 256z"></path></g></svg>`,
         `<svg width="32px" height="32px" viewBox="0 0 256 256" xmlns="http://www.w3.org/2000/svg" fill="#890505"><g id="SVGRepo_bgCarrier" stroke-width="0"></g><g id="SVGRepo_tracerCarrier" stroke-linecap="round" stroke-linejoin="round"></g><g id="SVGRepo_iconCarrier"><path fill="#000000" d="M238.7215 147.5715a52.225 52.225 90 01-101.13 18.335c-.04 34.365 2.165 57.23 34.775 74.5h-88.785c32.61-17.265 34.815-40.125 34.775-74.5a52.205 52.205 90 11-33.17-68.14 52.225 52.225 90 1185.57 0 52.25 52.25 90 0167.965 49.805z"></path></g></svg>
`
        ]
    let suitsvg = $derived(suitSVG[Math.floor(cardval / 13)]);
</script>

<div class="flex p-1">
    <div class="flex flex-col w-30 h-45 rounded-xl transition-all border-4 border-gray-900 bg-gray-400">
        <h1 class="text-black text-2xl font-bold p-2">{card}</h1>
        <div class="flex w-full justify-center py-5">
            {@html suitsvg}
        </div>
        
        
        
    </div>
</div>
