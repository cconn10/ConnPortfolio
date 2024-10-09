<script lang="ts">
    import '@fortawesome/fontawesome-free/css/all.min.css'

    import { fly } from 'svelte/transition'
    import { elasticInOut } from 'svelte/easing'

    import {page} from '$app/stores'

    // function tooltip(node: any) {
    //     const tooltip = tippy(node);

    //     return {
    //         destroy() {
    //             tooltip.destroy();
    //         }
    //     }
    // }


    let fabOpen:boolean = false

    let fabItems = [
        {id: 0, name: "LinkedIn", iconClasses: "fa-brands fa-linkedin", href:"https://www.linkedin.com/in/colin-conn/" },
        {id: 1, name: "Github", iconClasses: "fa-brands fa-github", href:"https://github.com/cconn10"},
        {id: 2, name: "Codepen", iconClasses: "fa-brands fa-codepen", href:"https://codepen.io/cconn10"},
        {id: 3, name: "FreeCodeCamp", iconClasses: "fa-brands fa-free-code-camp", href:"https://www.freecodecamp.org/conncf6010"},
        {id: 4, name: "conncf@mail.uc.edu", iconClasses: "fa-regular fa-envelope", href:"mailto:conncf@mail.uc.edu"}
    ]

    let pages = [
        {text: "Projects", href:"/projects"},
        {text: "Experience", href:"/work-experience"},
        {text: "About", href:"/about"},
        {text: "Home", href:"/"}
    ]

</script>

<nav>
    <ul>
        <li class="navLeft"><a class="navItem" href="/">Colin Conn</li>
        {#each pages as {text, href}}
            <li class="navRight">
                <a class="navItem {href === $page.url.pathname ? 'active' : ''}" {href}>
                    {text}
                </a>
            </li>
        {/each}
    </ul>

</nav>

<slot></slot>

<button class="faButton" on:click="{() => fabOpen = !fabOpen}">
    <i class="fa-solid {fabOpen ? "fa-xmark" : "fa-link"}" />
</button>
<div class="fabMenu">
    {#if fabOpen}
        {#each fabItems as {id, name, iconClasses, href}}
        <a 
            class="fabItem" 
            {href}
            target="_blank" 
            in:fly|global={{ y: 65, duration: 200, delay:(200*id), easing:elasticInOut}}
            out:fly|global={{ y: 65, duration: 200, delay: (((fabItems.length-1)*200) - (200*id)), easing:elasticInOut}}>
                <i class="{iconClasses} fabIcon fa-xl"></i>
        </a>
        {/each}
    {/if}
</div>

<style>
    nav {
        height: 50px;
        margin: 20px;
    }
    
    ul {
        margin: 0;
        padding: 0;
        list-style-type: none;
    }

    .navLeft {
        float: left;
    }

    .navRight {
        float: right;
        padding-left: 30px;
    }

    .navItem {
        font-size: 24px;
    }

    .navItem, .fabItem {
        font-weight: 600;
        text-decoration: none;

        color: var(--primary);
    }

    a:hover, button:hover, a.active {
        filter: brightness(150%);
        cursor: pointer;
    }

    .active {
        text-decoration: wavy underline;
    }
    
    .fabMenu {
        position: fixed;
        bottom: 99px;
        right: 25px;

        display: flex;
        flex-direction: column-reverse;
        gap: 25px;

    }

    .fabItem {
        text-decoration: none;
        border-style: none;

        padding: 10px 10px;
    }

    .fabItem, .faButton {
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .faButton {
        position: fixed;
        bottom: 25px;
        right: 25px;

        width:50px;
        height:50px;

        border: 2px solid var(--primary);
        border-radius: 100%;

        background-color: transparent;
        color: var(--primary);
    }

    
</style>