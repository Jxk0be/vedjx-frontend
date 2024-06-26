<script>
    import "../app.css";
    import Logo from '$lib/data/LogoWhite.svg?raw';
    import Icon from '@iconify/svelte';

    let menuOpen = false;

    const handleClose = () => {
        menuOpen = false
        if (document.body.classList.contains("noScroll")) {
            document.body.classList.remove("noScroll")
        }
  
    }

    const handleMenu = () => {
        menuOpen = !menuOpen

        if (document.body.classList.contains("noScroll")) {
            document.body.classList.remove("noScroll")
        }
        else {
            document.body.classList.add("noScroll")
        }
    }

</script>

<div class="w-full flex items-center fixed z-40 custom-gradient">
    <div class="mx-auto max-w-[1500px] px-9 w-full">
        <div class="flex justify-between text-[20px] items-center font-semibold w-full h-[140px]">
            <a class="font-bold min-w-[200px] py-[10px]" href="/">
                {@html Logo}
            </a>
            <div class="w-auto h-full lg:flex hidden justify-between items-center">
                <a href="/">Home</a>
                <a class="ml-7" href="/projects">Our Work</a>
                <a class="ml-7" href="/services">Services</a>
                <a class="ml-7" href="/about">About</a>
                <a class="ml-7" href="/free-quote">Free Quote</a>
                <a class="ml-7" href="/register">Register</a>
                <a class="ml-7" href="/login">Login</a>
            </div>
            <button class={`lg:hidden md:text-[45px] text-[40px] text-white transition-all ease-in-out rounded-xl duration-150 ${!menuOpen ? '' : 'bg-[#0b0b0b] p-3'}`} on:click={() => handleMenu()} >
                <Icon icon="lucide:menu" />
            </button>
        </div>
    </div>
</div>


{#if menuOpen}
    <button class={`${menuOpen ? 'left-0' : 'left-[-100%]'} lg:hidden z-20 overflow-x-hidden overflow-y-hidden w-full cursor-auto h-screen fixed top-0 left-0 bg-[#222222]/50`} on:click={() => handleMenu()}></button>
{/if}

<div class={`transition-all z-30 text-[105%] uppercase custom-gradient font-semibold min-w-[250px] text-white lg:hidden ease-in-out duration-300 ${menuOpen ? 'right-0' : 'md:right-[-40%] right-[-250px]'} fixed pt-[140px] w-[250px] top-0 md:w-[40%] h-full border-l border-gray-600`}>
    <ul class="text-left font-bold">
        <a href="/" on:click={() => handleClose()}>
            <li class="p-5 border-b border-gray-600 duration-150 ease-in-out transition-all hover:text-[#61BEFF]">Home</li>
        </a>
        <a href="/projects" on:click={() => handleClose()}>
            <li class="p-5 border-b border-gray-600 duration-150 ease-in-out transition-all hover:text-[#61BEFF]">Our Work</li>
        </a>
        <a href="/services" on:click={() => handleClose()}>
            <li class="p-5 border-b border-gray-600 duration-150 ease-in-out transition-all hover:text-[#61BEFF]">Services</li>
        </a>
        <a href="/about" on:click={() => handleClose()}>
            <li class="p-5 border-b border-gray-600 duration-150 ease-in-out transition-all hover:text-[#61BEFF]">About</li>
        </a>
        <a href="/free-quote" on:click={() => handleClose()}>
            <li class="p-5 border-b border-gray-600 duration-150 ease-in-out transition-all hover:text-[#61BEFF]">Free Quote</li>
        </a>
        <a href="/register" on:click={() => handleClose()}>
            <li class="p-5 border-b border-gray-600 duration-150 ease-in-out transition-all hover:text-[#61BEFF]">Register</li>
        </a>
        <a href="/login" on:click={() => handleClose()}>
            <li class="p-5 border-b border-gray-600 duration-150 ease-in-out transition-all hover:text-[#61BEFF]">Login</li>
        </a>
    </ul>
</div>


<div class="w-full min-h-screen flex items-center h-auto">
    <div class="mx-auto max-w-[1500px] md:px-3 w-full min-h-screen h-auto">
        <slot />
    </div>
</div>