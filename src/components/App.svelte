<script>
    import { onMount } from "svelte";
    import RawCode from "./RawCode.svelte";
    import Card1 from "./Card1.svelte";
    import Card2 from "./Card2.svelte";
    import { writable } from "svelte/store";

    let currentSection = writable("Background"); // Reactive store
    const sections = ["TODO","Background", "Leave One Out","Discretization Methods"];

    onMount(() => {
        const observer = new IntersectionObserver(
            (entries) => {
                entries.forEach((entry) => {
                    if (entry.isIntersecting) {
                        currentSection.set(entry.target.id); // Update reactively
                    }
                });
            },
            { threshold: 0.5 }
        );

        document.querySelectorAll("section").forEach((section) => {
            observer.observe(section);
        });
    });

    function scrollToSection(section) {
        document.getElementById(section).scrollIntoView({ behavior: "smooth" });
    }
</script>

<main>
    <nav>
        <ul>
            {#each sections as section}
                <li on:click={() => scrollToSection(section)} class="{$currentSection === section ? 'active' : ''}">
                    {section}
                </li>
            {/each}
        </ul>
    </nav>

    <section id="TODO" style="min-height: 50vh;">
        <h1>TODO</h1>
        <p>
            This presentation focuses on exploring techniques and fill-in-word practices for ensuring data robustness.
            TODO: PATTERN MINING(how it works, results of pattern mining, lineplots heatmaps showcase, insurance is exception(super sensitive)), and pattern mining results,
            normalization(normalize all results togeter, which is more robust in respect to a parameter, predicting a specific column),  
        </p>
    </section>

    <section id="Background" style="min-height: 50vh;">
        <h1>Background</h1>
        <p>
            Pattern mining in this study is used to identify key structures within a dataset, 
            improving robustness measurement and error injection techniques. 
            The proposed method builds on previous work by learning dataset patterns efficiently and consistently across different datasets. 
            The process involves training a classifier, injecting errors into the data, applying robustness methods (favoring ZORRO over Meyer), 
            and evaluating robustness through numerical ratios and visualizations. 
            The heuristic pattern mining approach captures all possible predicates and inequalities, filtering out less relevant ones to refine error injection strategies. 
            This allows for a more effective robustness classifier that standardizes comparisons across datasets. 
        </p>
        <RawCode  />
        
        
        

    </section>

    <section id="Leave One Out">
        <h1>Leave One Out</h1>
        
        <p>
            This following are the results of utilizing the leave one out method mentioned in section 2.1
to analyze the robustness ratios under given parameters for the below 2 datasets. Note per
section 2.3, robustness radius is declared for both datasets already with the MPG dataset
using a parameter of 2 and the insurance dataset using a parameter of 500. We’ll go into
further explanation behind the reasoning of setting these parameters at these key values in
the discussion section.
        </p>
        <Card1 />
    </section>


    <section id="Discretization Methods">
        <h1>Discretization Methods</h1>
        
        <p>
            The following are the results of utilizing the discretization method mentioned in section 2.2
to analyze the robustness ratios under given parameters for the 2 datasets utilized in 3.1.1.

ZORRO is more consistent in maintaining a high robustness ratio across all conditions, 
while Meyer’s method shows greater variability, ranging from 100% to as low as 18.4%.
This suggests significant weaknesses in Meyer’s approach under certain conditions.
Based on the graph, ZORRO appears to be the more robust algorithm, consistently outperforming Meyer in robustness testing.
        </p>
        <Card2 />
    </section>


</main>

<style>
    @import url('https://fonts.googleapis.com/css2?family=Merriweather:ital,wght@0,300;0,400;0,700;0,900;1,300;1,400;1,700;1,900&display=swap');
    
    :root {
        --color-bg: rgb(0, 0, 0);
        --color-text: rgb(255, 255, 255);
        --nav-bg: rgb(255, 225, 0);
        --nav-text: rgb(0, 0, 0);
        --nav-active: rgb(0, 0, 0);
    }

    :global(body) {
        background-color: var(--color-bg);
    }

    *,
    *::before,
    *::after {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
    }

    body {
        background-color: var(--color-bg);
    }

    nav {
        background-color: var(--nav-bg);
        color: var(--nav-text);
        padding: 12px 24px;
        position: fixed;
        top: 0;
        left: 0;
        right: 0;
        box-shadow: 0 25px 10px rgba(0, 0, 0, 0.75);
        z-index: 10;
        display: flex;
        justify-content: center;
    }

    nav ul {
        list-style: none;
        display: flex;
        gap: 20px;
    }

    nav li {
        font-family: 'Merriweather', serif;
        font-size: 22px;
        font-weight: 700;
        cursor: pointer;
        transition: color 0.3s ease, border 0.3s ease, background-color 0.3s ease;
    }

    nav li.active {
        color: var(--nav-active);
        border: 3px solid var(--nav-active);
        border-radius: 10px;
        background-color: rgb(255, 255, 255);
        padding: 3px 10px;
    }

    main {
        height: 100vh;
        padding: 80px 20px 20px;
        padding-top: 100px;
        font-family: 'Merriweather', serif;
        font-weight: 300;
        line-height: 1.5;
        font-size: 24px;
        color: var(--color-text);
        text-align: center;
    }

    section {
        padding: 20px 0; /* Ensures sections are large enough for the observer */
        min-height: 40vh; /* Adjust for better scrolling detection */
    }

    h1 {
        font-size: 40px;
        font-weight: 300;
        line-height: 1.2;
        margin-bottom: 20px;
        text-align: left;
        padding-left: 20px;
    }

    p {
        font-size: 20px;
        font-weight: 400;
        line-height: 1.6;
        text-align: left;
        padding-left: 20px;
    }
</style>
