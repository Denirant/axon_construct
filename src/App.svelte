<script>
    localStorage.setItem("theme", "light");
</script>

<main class="bg-white">
    <div
        class="relative flex h-screen w-full items-center justify-center bg-white"
    >
        <div
            class="
            opacity-60 absolute inset-0 [background-size:30px_30px] sm:[background-size:60px_60px] [background-image:linear-gradient(to_right,#e4e4e4_1px,transparent_1px),linear-gradient(to_bottom,#e4e4e4_1px,transparent_1px)]
        "
        />
        <div
            class="pointer-events-none absolute inset-0 flex items-center justify-center bg-white [mask-image:radial-gradient(60vh,transparent_10%,black)] sm:[mask-image:radial-gradient(60vw,transparent_10%,black)]"
        />
        <div class="flex flex-col justify-between py-7.5 items-center h-screen">
            <p
                class="select-none inset-0 relative z-20 text-sm sm:text-2xl flex gap-1 sm:gap-3 items-center font-primary font-medium text-neutral-700 tracking-wider bg-white py-1 sm:py-2 px-2 sm:px-3 pr-4 sm:pr-5 rounded-full border border-gray-100/70"
            >
                <img
                    src="favicon.png"
                    alt="logo"
                    class="size-6 sm:size-10 rounded-full"
                /> AxonAI
            </p>
            <div
                class="select-none inset-0 relative z-20 text-center gap-6 sm:gap-4 flex flex-col -mt-10"
            >
                <p
                    class="text-xl sm:text-5xl font-bold sm:font-medium font-mono leading-9 sm:leading-19 text-gray-900 trcking"
                >
                    🚧 Oops! We're having <br /> a little construction party!
                </p>
                <span
                    class="block text-xs sm:text-lg text-neutral-600/70 font-mono font-medium w-[80%] leading-5 sm:w-[60%] sm:leading-10 mx-auto"
                >
                    We're cooking up something awesome (and making a bit of a
                    mess in the process). Thanks for your patience — we promise
                    it'll be worth the wait!
                </span>
            </div>

            <div
                class="relative inset-0 z-20 flex flex-col gap-4 items-center mb-4"
            >
                <div class="flex gap-2 items-center select-none">
                    <a
                        class="px-4 pr-5 py-2 rounded-xl sm:rounded-2xl bg-blue-500 hover:bg-blue-500/90 transition-colors duration-300 text-white flex text-semibold tracking-wider items-center gap-2.5 text-md sm:text-lg mb-12 sm:mb-8"
                        draggable="false"
                        href="https://t.me/axonai"
                        target="_blank"
                        ><img
                            src="https://img.icons8.com/fluent-systems-filled/512/FFFFFF/telegram-app.png"
                            alt="tg-icon"
                            class="size-5 sm:size-6 object-contain"
                        /> Telegram</a
                    >
                </div>

                <a
                    href="mailto:axonsupp@axonai.tech"
                    class="relative inset-0 text-gray-700/70 underline underline-offset-4 sm:underline-offset-5 text-xs sm:text-sm"
                    >axonsupp@axonai.tech
                </a>
            </div>
        </div>
    </div>
</main>
