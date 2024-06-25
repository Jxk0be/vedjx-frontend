<script>
    import "../app.css";
    import Logo from '$lib/data/Logo.svg?raw';
</script>

<div class="w-full flex items-center fixed z-20 custom-gradient">
    <div class="mx-auto max-w-[1500px] px-5 w-full">
        <div class="flex justify-between text-[20px]  items-center font-semibold w-full h-[70px]">
            <a class="font-bold min-w-[100px] text-xl" href="/">
                {@html Logo}
            </a>
            <div class="w-auto h-full flex justify-between items-center">
                <a href="/">Home</a>
                <a class="ml-7" href="/projects">Our Work</a>
                <a class="ml-7" href="/services">Services</a>
                <a class="ml-7" href="/about">About</a>
                <a class="ml-7" href="/free-quote">Free Quote</a>
                <a class="ml-7" href="/register">Register</a>
                <a class="ml-7" href="/login">Login</a>
            </div>
        </div>
    </div>
</div>

<div class="w-full min-h-screen flex items-center h-auto">
    <div class="mx-auto max-w-[1500px] px-3 w-full min-h-screen h-auto">
        <slot />
    </div>
</div>