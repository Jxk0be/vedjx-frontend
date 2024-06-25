<script>
    import "../app.css";
</script>

<div class="w-full flex items-center fixed">
    <div class="mx-auto max-w-[1500px] w-full">
        <div class="flex justify-between items-center w-full h-[70px] bg-red-500">
            <h1>Ved.jx</h1>
            <div class="w-7/12 h-full flex bg-slate-500 justify-between items-center">
                <a href="/">Home</a>
                <a href="/projects">Our Work</a>
                <a href="/services">Services</a>
                <a href="/about">About</a>
                <a href="/free-quote">Free Quote</a>
                <a href="/register">Register</a>
                <a href="/login">Login</a>
            </div>
        </div>
    </div>
</div>

<div class="w-full min-h-screen flex items-center h-auto">
    <div class="mx-auto max-w-[1500px] w-full min-h-screen h-auto">
        <slot />
    </div>
</div>