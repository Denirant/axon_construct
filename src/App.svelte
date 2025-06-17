<script>
    import { onMount } from "svelte";
    import { writable } from "svelte/store";
    import { ShieldX, MenuIcon, X } from "lucide-svelte";

    // State management
    const uiState = writable({
        mobileMenuOpen: false,
        currentDemo: 0,
        isScrolled: false,
        formSubmitting: false,
        formSuccess: false,
    });

    // Form data
    let formData = {
        name: "",
        email: "",
        company: "",
        message: "",
        interest: "general",
    };

    let formErrors = {};
    let newsletterEmail = "";
    let newsletterSuccess = false;

    export let lineCount = 10;
    export let radius = 12;
    export let animationDuration = 1.2;
    
    $: stepAngle = 360 / lineCount;
    $: delayStep = animationDuration / lineCount;
    
    $: lines = Array.from({ length: lineCount }, (_, i) => ({
      angle: i * stepAngle,
      delay: -(i * delayStep)
    }));
  
    // Content data
    const demoModes = [
        {
            type: "text",
            title: "Text Analysis",
            input: "Analyze this quarterly report and extract key insights...",
            output: "Key insights: Revenue up 23%, customer satisfaction improved by 15%, operational costs reduced by 8%. Recommended actions: expand marketing in Q3, optimize supply chain.",
            metrics: { time: "0.2s", accuracy: "99.1%", tokens: "1.2K" },
        },
        {
            type: "image",
            title: "Visual Understanding",
            input: "Processing uploaded architectural blueprint...",
            output: "Detected: 3-story residential building, 2,400 sq ft, 4 bedrooms, 3 bathrooms. Potential issues: HVAC placement, electrical load distribution. Compliance: meets local building codes.",
            metrics: { time: "0.8s", accuracy: "97.3%", objects: "47" },
        },
        {
            type: "audio",
            title: "Audio Intelligence",
            input: "Transcribing 45-minute board meeting recording...",
            output: "Meeting summary: 5 key decisions made, 12 action items assigned, budget approved for Q4 initiatives. Sentiment: positive (78%), concerns raised about timeline feasibility.",
            metrics: { time: "1.2s", accuracy: "98.7%", speakers: "7" },
        },
    ];

    const features = [
        {
            icon: `<svg class="w-8 h-8" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z"></path></svg>`,
            title: "Multi-Modal Processing",
            description:
                "Process text, images, audio, and video with state-of-the-art AI models in a unified platform.",
            benefits: [
                "50+ file formats supported",
                "Real-time processing",
                "Batch operations",
            ],
        },
        {
            icon: `<svg class="w-8 h-8" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M13 10V3L4 14h7v7l9-11h-7z"></path></svg>`,
            title: "Lightning Performance",
            description:
                "Sub-second response times powered by optimized inference engines and global edge networks.",
            benefits: [
                "< 300ms average response",
                "99.9% uptime SLA",
                "Global CDN",
            ],
        },
        {
            icon: `<svg class="w-8 h-8" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M12 15v2m-6 4h12a2 2 0 002-2v-6a2 2 0 00-2-2H6a2 2 0 00-2 2v6a2 2 0 002 2zm10-10V7a4 4 0 00-8 0v4h8z"></path></svg>`,
            title: "Enterprise Security",
            description:
                "Bank-grade security with end-to-end encryption, SOC 2 compliance, and GDPR adherence.",
            benefits: [
                "AES-256 encryption",
                "SOC 2 Type II certified",
                "GDPR compliant",
            ],
        },
        {
            icon: `<svg class="w-8 h-8" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M8 9l3 3-3 3m5 0h3M5 20h14a2 2 0 002-2V6a2 2 0 00-2-2H5a2 2 0 00-2 2v14a2 2 0 002 2z"></path></svg>`,
            title: "Developer-First API",
            description:
                "Comprehensive REST and GraphQL APIs with SDKs for all major programming languages.",
            benefits: [
                "RESTful & GraphQL APIs",
                "10+ language SDKs",
                "Extensive documentation",
            ],
        },
        {
            icon: `<svg class="w-8 h-8" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M9 19v-6a2 2 0 00-2-2H5a2 2 0 00-2 2v6a2 2 0 002 2h2a2 2 0 002-2zm0 0V9a2 2 0 012-2h2a2 2 0 012 2v10m-6 0a2 2 0 002 2h2a2 2 0 002-2m0 0V5a2 2 0 012-2h2a2 2 0 012 2v14a2 2 0 01-2 2h-2a2 2 0 01-2-2z"></path></svg>`,
            title: "Smart Analytics",
            description:
                "Deep insights into usage patterns, performance metrics, and cost optimization opportunities.",
            benefits: [
                "Real-time dashboards",
                "Custom reports",
                "Cost optimization",
            ],
        },
        {
            icon: `<svg class="w-8 h-8" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M4.318 6.318a4.5 4.5 0 000 6.364L12 20.364l7.682-7.682a4.5 4.5 0 00-6.364-6.364L12 7.636l-1.318-1.318a4.5 4.5 0 00-6.364 0z"></path></svg>`,
            title: "Custom Training",
            description:
                "Fine-tune models on your specific data for enhanced accuracy and domain expertise.",
            benefits: [
                "Custom model training",
                "Domain adaptation",
                "Private deployments",
            ],
        },
    ];

    const pricingPlans = [
        {
            name: "Starter",
            price: "$29",
            period: "/month",
            description: "Perfect for small teams and proof of concepts",
            features: [
                "10K API calls/month",
                "Text & image processing",
                "Standard support",
                "Basic analytics",
                "5 team members",
            ],
            cta: "Start free trial",
            popular: false,
        },
        {
            name: "Professional",
            price: "$99",
            period: "/month",
            description: "For growing businesses with advanced needs",
            features: [
                "100K API calls/month",
                "All content types",
                "Priority support",
                "Advanced analytics",
                "Unlimited team members",
                "Custom integrations",
            ],
            cta: "Start free trial",
            popular: true,
        },
        {
            name: "Enterprise",
            price: "Custom",
            period: "pricing",
            description: "For large organizations with specific requirements",
            features: [
                "Unlimited API calls",
                "Custom model training",
                "Dedicated support",
                "White-label options",
                "On-premise deployment",
                "SLA guarantees",
            ],
            cta: "Contact sales",
            popular: false,
        },
    ];

    const roadmapItems = [
        {
            quarter: "Q1 2024",
            title: "Foundation Launch",
            status: "completed",
            items: [
                "Core multi-modal AI engine",
                "REST API v1.0",
                "Web dashboard",
                "Basic authentication",
            ],
            progress: 100,
        },
        {
            quarter: "Q2 2024",
            title: "Enhanced Capabilities",
            status: "current",
            items: [
                "Real-time streaming API",
                "Advanced audio processing",
                "Video analysis (beta)",
                "Mobile SDKs",
            ],
            progress: 75,
        },
        {
            quarter: "Q3 2024",
            title: "Enterprise Ready",
            status: "upcoming",
            items: [
                "Single Sign-On (SSO)",
                "Advanced security features",
                "Custom model training",
                "Workflow automation",
            ],
            progress: 25,
        },
        {
            quarter: "Q4 2024",
            title: "AI Evolution",
            status: "planned",
            items: [
                "GPT-5 integration",
                "Edge computing support",
                "Multi-modal fusion AI",
                "Autonomous agents",
            ],
            progress: 0,
        },
    ];

    const testimonials = [
        {
            quote: "OmniMind transformed our content analysis workflow. What used to take hours now happens in minutes with incredible accuracy.",
            author: "Sarah Chen",
            title: "VP of Data Science",
            company: "TechFlow Inc.",
            avatar: "SC",
        },
        {
            quote: "The API integration was seamless. Our development team had it up and running in production within a week.",
            author: "Marcus Rodriguez",
            title: "Lead Engineer",
            company: "DataStream Solutions",
            avatar: "MR",
        },
        {
            quote: "Finally, an AI platform that actually understands our business context. The multi-modal capabilities are game-changing.",
            author: "Emily Watson",
            title: "Chief Innovation Officer",
            company: "InnovateCorp",
            avatar: "EW",
        },
    ];

    // Utility functions
    function validateEmail(email) {
        return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
    }

    function validateForm() {
        const errors = {};

        if (!formData.name.trim()) {
            errors.name = "Name is required";
        }

        if (!formData.email.trim()) {
            errors.email = "Email is required";
        } else if (!validateEmail(formData.email)) {
            errors.email = "Please enter a valid email address";
        }

        if (!formData.message.trim()) {
            errors.message = "Message is required";
        } else if (formData.message.trim().length < 10) {
            errors.message = "Message must be at least 10 characters";
        }

        return errors;
    }

    async function submitForm() {
        formErrors = validateForm();

        if (Object.keys(formErrors).length === 0) {
            uiState.update((state) => ({ ...state, formSubmitting: true }));

            try {
                // In production, replace with actual API call
                const response = await fetch("/api/contact", {
                    method: "POST",
                    headers: {
                        "Content-Type": "application/json",
                    },
                    body: JSON.stringify(formData),
                });

                if (response.ok) {
                    uiState.update((state) => ({
                        ...state,
                        formSuccess: true,
                        formSubmitting: false,
                    }));
                    formData = {
                        name: "",
                        email: "",
                        company: "",
                        message: "",
                        interest: "general",
                    };

                    // Reset success message after 5 seconds
                    setTimeout(() => {
                        uiState.update((state) => ({
                            ...state,
                            formSuccess: false,
                        }));
                    }, 5000);
                } else {
                    throw new Error("Form submission failed");
                }
            } catch (error) {
                console.error("Form submission error:", error);
                alert(
                    "There was an error submitting your form. Please try again."
                );
                uiState.update((state) => ({
                    ...state,
                    formSubmitting: false,
                }));
            }
        }
    }

    async function subscribeNewsletter() {
        if (!validateEmail(newsletterEmail)) {
            alert("Please enter a valid email address");
            return;
        }

        try {
            // In production, replace with actual API call
            const response = await fetch("/api/newsletter", {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                },
                body: JSON.stringify({ email: newsletterEmail }),
            });

            if (response.ok) {
                newsletterSuccess = true;
                newsletterEmail = "";
                setTimeout(() => {
                    newsletterSuccess = false;
                }, 3000);
            }
        } catch (error) {
            console.error("Newsletter subscription error:", error);
            alert("Error subscribing to newsletter. Please try again.");
        }
    }

    function scrollToSection(sectionId) {
        const element = document.getElementById(sectionId);
        if (element) {
            element.scrollIntoView({ behavior: "smooth" });
        }
        uiState.update((state) => ({ ...state, mobileMenuOpen: false }));
    }

    // Lifecycle
    onMount(() => {
        // Handle scroll for header background
        const handleScroll = () => {
            uiState.update((state) => ({
                ...state,
                isScrolled: window.scrollY > 10,
            }));
        };

        window.addEventListener("scroll", handleScroll);

        // Auto-rotate demo every 5 seconds
        const demoInterval = setInterval(() => {
            uiState.update((state) => ({
                ...state,
                currentDemo: (state.currentDemo + 1) % demoModes.length,
            }));
        }, 5000);

        // Intersection Observer for animations
        const observerOptions = {
            threshold: 0.1,
            rootMargin: "0px 0px -50px 0px",
        };

        const observer = new IntersectionObserver((entries) => {
            entries.forEach((entry) => {
                if (entry.isIntersecting) {
                    entry.target.classList.add("animate-fade-in");
                }
            });
        }, observerOptions);

        document
            .querySelectorAll(".observe")
            .forEach((el) => observer.observe(el));

        return () => {
            window.removeEventListener("scroll", handleScroll);
            clearInterval(demoInterval);
            observer.disconnect();
        };
    });

    $: currentState = $uiState;
    $: currentDemo = demoModes[currentState.currentDemo];
</script>

<svelte:head>
    <title>AxonAI - Multi-Modal AI Platform</title>
    <meta
        name="description"
        content="Axon AI — One platform for text, image, audio, and video intelligence.
        Create smarter. Move faster."
    />
    <meta
        name="keywords"
        content="AI, machine learning, multi-modal AI, enterprise AI, API"
    />
    <meta
        property="og:title"
        content="Axon AI - Revolutionary Multi-Modal AI Platform"
    />
    <meta
        property="og:description"
        content="AxonAI. Create smarter. Move faster."
    />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <link rel="canonical" href="https://axonai.tech" />
</svelte:head>

<div class="relative min-h-screen max-h-screen overflow-hidden">
    <div class="blob blob-1"></div>
    <div class="blob blob-2"></div>
    <div class="blob blob-3"></div>
    <div class="blob blob-4"></div>

    <!-- Header -->
    <!-- <header
        class="fixed top-0 z-50 transition-all duration-300 ease-out {currentState.isScrolled
            ? 'bg-white/60 backdrop-blur-sm border border-gray-50 mt-3 mx-3 w-[calc(100%-1.5rem)] rounded-4xl'
            : 'rounded-none border-gray-50/0 bg-transparent w-full'}"
    >
        <nav class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-6">
            <div class="flex justify-between items-center h-20">
                <div
                    class="flex items-center space-x-3 group cursor-pointer"
                    on:click={() =>
                        window.scrollTo({ top: 0, behavior: "smooth" })}
                >
                    <div
                        class="size-8 rounded-full flex items-center justify-center group-hover:scale-105 transition-transform"
                    >
                        <img src="./favicon.png" alt="" />
                    </div>
                    <span
                        class="text-xl font-medium text-neutral-800 tracking-tight"
                    >
                        Axon AI
                    </span>
                </div>

                <div class="hidden md:flex items-center space-x-7">
                    <button
                        on:click={() => scrollToSection("features")}
                        class="text-sm text-neutral-800 hover:text-neutral-600/80 font-medium transition-colors cursor-pointer"
                    >
                        About
                    </button>
                    <button
                        on:click={() => scrollToSection("pricing")}
                        class="text-sm text-neutral-800 hover:text-neutral-600/80 font-medium transition-colors cursor-pointer"
                    >
                        Features
                    </button>
                    <button
                        on:click={() => scrollToSection("roadmap")}
                        class="text-sm text-neutral-800 hover:text-neutral-600/80 font-medium transition-colors cursor-pointer"
                    >
                        Pricing
                    </button>
                    <button
                        on:click={() => scrollToSection("contact")}
                        class="text-sm text-neutral-800 hover:text-neutral-600/80 font-medium transition-colors cursor-pointer"
                    >
                        Contact
                    </button>
                    <div class="flex items-center space-x-2">
                        <button
                            class="bg-neutral-800 text-white text-sm px-5 py-2.25 rounded-2xl font-medium hover:bg-neutral-800/90 hover:text-white/90 transition-all duration-300 cursor-pointer"
                        >
                            Get started
                        </button>
                    </div>
                </div>

                <button
                    class="md:hidden p-2 rounded-2xl text-neutral-500 hover:text-neutral-800 hover:bg-white/40 border border-transparent hover:border-neutral-50/50 cursor-pointer duration-200 transition-colors"
                    on:click={() =>
                        uiState.update((state) => ({
                            ...state,
                            mobileMenuOpen: !state.mobileMenuOpen,
                        }))}
                >
                    {#if !currentState.mobileMenuOpen}
                        <MenuIcon />
                    {:else}
                        <X />
                    {/if}
                </button>
            </div>

            {#if currentState.mobileMenuOpen}
                <div
                    class="md:hidden p-2 absolute top-18 right-4 w-60 rounded-3xl border-gray-200 bg-white backdrop-blur-xl"
                >
                    <div class="flex flex-col space-y-2">
                        <button
                            on:click={() => scrollToSection("features")}
                            class="text-left text-gray-600 hover:text-gray-800 font-medium bg-white hover:bg-gray-200/50 cursor-pointer transition-colors duration-200 rounded-3xl px-4 py-2"
                            >Features</button
                        >
                        <button
                            on:click={() => scrollToSection("pricing")}
                            class="text-left text-gray-600 hover:text-gray-800 font-medium bg-white hover:bg-gray-200/50 cursor-pointer transition-colors duration-200 rounded-3xl px-4 py-2"
                            >Pricing</button
                        >
                        <button
                            on:click={() => scrollToSection("roadmap")}
                            class="text-left text-gray-600 hover:text-gray-800 font-medium bg-white hover:bg-gray-200/50 cursor-pointer transition-colors duration-200 rounded-3xl px-4 py-2"
                            >Roadmap</button
                        >
                        <button
                            on:click={() => scrollToSection("contact")}
                            class="text-left text-gray-600 hover:text-gray-800 font-medium bg-white hover:bg-gray-200/50 cursor-pointer transition-colors duration-200 rounded-3xl px-4 py-2"
                            >Contact</button
                        >
                        <div class="mt-1 pt-3 border-t border-gray-200/50 space-y-3">
                            <button
                                class="w-full bg-neutral-800 px-4 py-2 text-white hover:bg-neutral-700 hover:text-neutral-100 duration-200 cursor-pointer transition-colors rounded-3xl font-medium"
                            >
                                Get started
                            </button>
                        </div>
                    </div>
                </div>
            {/if}
        </nav>
    </header> -->

    <!-- Hero Section -->
    <section
        class="relative h-screen flex flex-col items-center justify-center px-4 sm:px-6 lg:px-8 overflow-hidden"
    >
        <!-- Background gradients -->

        <div class="relative max-w-7xl mx-auto h-full">
            <div
                class="flex flex-col items-center justify-center h-full text-center max-w-5xl py-2"
            >
                <!-- Main headline -->
                <!-- <h1
                    class="text-3xl md:text-5xl font-bold text-neutral-800 tracking-tight mb-8 observe"
                >
                    <span class=""> Axon Studio Dev </span>
                </h1> -->

                <!-- <p
                    class="absolute w-94 left-1/2 -translate-x-1/2 bottom-20 bg-red-50/50 px-4 py-1.5 rounded-2xl border border-red-100/50 text-red-500/80 font-medium text-[14px] mt-4 tracking-wide flex items-center gap-1.5"
                >
                    <ShieldX
                        class="size-4.5 text-red-500/80 fill-red-100/80"
                        stroke-width={1.5}
                    /> Toolchain init failed, see `/var/mcp/query.log`
                </p> -->
                <div class="flex items-center justify-center min-h-screen">
                    <div class="relative">
                      <!-- Динамический iOS Activity Indicator -->
                      <div class="relative w-6 h-6 left-3">
                        {#each lines as line, i}
                          <div 
                            class="absolute w-0.5 h-1.75 bg-gray-400 rounded-sm origin-center animate-ios-fade" 
                            style="transform: rotate({line.angle}deg); animation-delay: {line.delay}s; transform-origin: center {radius}px;"
                          ></div>
                        {/each}
                      </div>
                    </div>
                  </div>
                <p
                    class="absolute bottom-4 text-xs text-gray-500 font-mono tracking-tight"
                >
                    Release 0.14.0.b
                </p>
            </div>
        </div>
    </section>
</div>

<style>
    /* Custom animations */
    @keyframes fade-in {
        from {
            opacity: 0;
            transform: translateY(30px);
        }
        to {
            opacity: 1;
            transform: translateY(0);
        }
    }

    .animate-fade-in {
        animation: fade-in 0.6s ease-out forwards;
    }

    /* Grid pattern background */
    .bg-grid-pattern {
        background-image: radial-gradient(circle, #e5e7eb 1px, transparent 1px);
        background-size: 20px 20px;
    }

    /* Smooth scrolling */
    html {
        scroll-behavior: smooth;
    }

    /* Form focus states */
    input:focus,
    textarea:focus,
    select:focus {
        box-shadow: 0 0 0 3px rgba(99, 102, 241, 0.1);
    }

    /* Button hover effects */
    button {
        transform-origin: center;
    }

    /* Responsive text scaling */
    @media (max-width: 640px) {
        .text-5xl {
            font-size: 2.5rem;
        }
        .text-7xl {
            font-size: 3.5rem;
        }
    }
    .blob {
        position: absolute;
        border-radius: 50%;
        opacity: 0.2;
        mix-blend-mode: screen;
        filter: blur(100px);
        animation: morph 25s ease-in-out infinite alternate;
        z-index: -1;
    }

    .blob-1 {
        background: radial-gradient(circle at 30% 30%, #ff4ecd, #7b2ff7);
        width: 600px;
        height: 600px;
        top: -50px;
        left: 100px;
        animation-delay: 0s;
    }

    .blob-2 {
        background: radial-gradient(circle at 70% 70%, #00ffe0, #0072ff);
        width: 500px;
        height: 500px;
        bottom: -150px;
        right: -200px;
        animation-delay: 3s;
    }

    .blob-3 {
        background: radial-gradient(circle at 50% 50%, #ffc800, #ff6f00);
        width: 450px;
        height: 450px;
        top: 35%;
        left: -100px;
        animation-delay: 6s;
    }

    .blob-4 {
        background: radial-gradient(circle at 50% 50%, #00ff8c, #00c2ff);
        width: 400px;
        height: 400px;
        bottom: 5%;
        left: 60%;
        animation-delay: 9s;
    }

    @keyframes morph {
        0% {
            transform: scale(1) translate(0, 0) rotate(0deg);
            border-radius: 42% 58% 67% 33% / 33% 33% 67% 67%;
        }
        25% {
            transform: scale(1.1) translate(20px, -25px) rotate(45deg);
            border-radius: 50% 50% 40% 60% / 60% 40% 60% 40%;
        }
        50% {
            transform: scale(1.3) translate(-30px, 30px) rotate(90deg);
            border-radius: 60% 40% 40% 60% / 60% 60% 40% 40%;
        }
        75% {
            transform: scale(1.1) translate(25px, 10px) rotate(135deg);
            border-radius: 35% 65% 65% 35% / 35% 35% 65% 65%;
        }
        100% {
            transform: scale(1) translate(-20px, 0px) rotate(180deg);
            border-radius: 42% 58% 67% 33% / 33% 33% 67% 67%;
        }
    }



    .animate-ios-fade {
      animation: ios-fade var(--duration, 1.2s) linear infinite;
    }
    
    @keyframes ios-fade {
      0% { opacity: 1; }
      100% { opacity: 0; }
    }
</style>
