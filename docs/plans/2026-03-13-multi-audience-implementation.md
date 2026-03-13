# Multi-Audience Site Redesign — Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Rewrite the Safe4Seniors marketing site from investor-heavy to family-first, serving families, B2B partners, and investors.

**Architecture:** Single-page HTML rewrite. Replace middle sections (lines 354-1100) of `public/index.html` while keeping head/meta (lines 1-265), nav structure (lines 266-352), scripts (lines 1137-1281), and the Demos section (lines 507-582) intact. Update nav links and footer to match new section IDs.

**Tech Stack:** Tailwind CDN, vanilla JS, Inter font (Google Fonts). No build step. Brand colors: brand-blue #1e3a5f, brand-blue-light #2c5282, brand-green #38a169, brand-gold #d69e2e.

**Design doc:** `docs/plans/2026-03-13-multi-audience-site-redesign.md`

---

## Line Map (current public/index.html, 1281 lines)

| Lines | Section | Action |
|-------|---------|--------|
| 1-265 | Head, meta, styles | Update meta description + OG tags |
| 266-352 | Nav | Update links to new section IDs |
| 354-396 | Hero | Rewrite |
| 398-407 | Dual-Pilot Banner | Delete |
| 409-505 | Solution (Problem/What We Do/Why Now) | Replace with How It Works |
| 507-582 | Demos (video cards) | Keep unchanged |
| 584-628 | Platform Ready | Replace with Technology |
| 629-746 | Dual-Pilot Details | Delete (absorbed into For Investors/Partners) |
| 748-939 | Business Model | Replace with For Partners + For Investors |
| 941-1004 | Partners (old 3-card) | Replace with Dual Mission |
| 1006-1066 | Team | Update titles/bios |
| 1068-1099 | Contact/CTA | Rewrite |
| 1101-1135 | Footer | Update nav links |
| 1137-1281 | Scripts | Keep unchanged |

---

### Task 1: Update Nav Links

**Files:**
- Modify: `public/index.html:266-352` (nav section)

**Step 1: Update desktop nav links (line ~316-325)**

Replace the desktop nav `<div>` contents with:

```html
<a href="#how-it-works" class="nav-link text-gray-600 hover:text-brand-blue font-medium">How It Works</a>
<a href="#demos" class="nav-link text-gray-600 hover:text-brand-blue font-medium">Demos</a>
<a href="#partners" class="nav-link text-gray-600 hover:text-brand-blue font-medium">Partners</a>
<a href="#investors" class="nav-link text-gray-600 hover:text-brand-blue font-medium">Investors</a>
<a href="#team" class="nav-link text-gray-600 hover:text-brand-blue font-medium">Team</a>
<a href="#contact" class="btn-primary bg-brand-blue text-white px-5 py-2.5 rounded-lg font-semibold hover:bg-brand-blue-light">
    Contact Us
</a>
```

**Step 2: Update mobile nav links (line ~340-347)**

Replace the mobile menu `<div>` contents with:

```html
<a href="#how-it-works" class="block px-4 py-3 text-gray-600 hover:bg-gray-50 rounded-lg font-medium">How It Works</a>
<a href="#demos" class="block px-4 py-3 text-gray-600 hover:bg-gray-50 rounded-lg font-medium">Demos</a>
<a href="#partners" class="block px-4 py-3 text-gray-600 hover:bg-gray-50 rounded-lg font-medium">Partners</a>
<a href="#investors" class="block px-4 py-3 text-gray-600 hover:bg-gray-50 rounded-lg font-medium">Investors</a>
<a href="#team" class="block px-4 py-3 text-gray-600 hover:bg-gray-50 rounded-lg font-medium">Team</a>
<a href="#contact" class="block px-4 py-3 bg-brand-blue text-white rounded-lg font-semibold text-center mt-2">Contact Us</a>
```

**Step 3: Verify no broken anchor links**

Run: `grep -n 'href="#' public/index.html | grep -v 'how-it-works\|demos\|partners\|investors\|team\|contact\|"#"'`
Expected: No results (all anchors point to valid section IDs)

**Step 4: Commit**

```bash
git add public/index.html
git commit -m "feat: update nav links for multi-audience redesign"
```

---

### Task 2: Rewrite Hero Section

**Files:**
- Modify: `public/index.html:354-396` (Hero section)

**Step 1: Replace Hero section (lines 354-396)**

Replace the entire Hero section with:

```html
    <!-- Hero Section -->
    <section class="gradient-hero text-white pt-28 pb-20 px-4 relative overflow-hidden">
        <!-- Background decoration -->
        <div class="absolute inset-0 opacity-10">
            <div class="absolute top-20 left-10 w-64 h-64 rounded-full bg-white blur-3xl"></div>
            <div class="absolute bottom-10 right-10 w-96 h-96 rounded-full bg-white blur-3xl"></div>
        </div>

        <div class="max-w-6xl mx-auto text-center relative z-10">
            <h1 class="text-4xl md:text-6xl font-extrabold mb-5 tracking-tight">
                Your loved one.<br>Checked on. <span class="text-brand-gold">Every day.</span>
            </h1>
            <p class="text-xl md:text-2xl text-blue-100 mb-3 font-medium">
                AI-powered daily wellness calls for seniors aging in place — no app, no internet required.
            </p>
            <p class="text-base text-blue-200/70 max-w-2xl mx-auto mb-10 leading-relaxed">
                Also preserving the Nimipuut&iacute;mt language with the Nez Perce Tribe.
            </p>

            <div class="flex flex-col sm:flex-row gap-4 justify-center mb-16">
                <a href="#how-it-works" class="btn-primary inline-block bg-brand-gold text-white px-8 py-4 rounded-lg font-bold text-lg hover:bg-brand-gold-dark">
                    See How It Works
                </a>
                <a href="#partners" class="inline-block border-2 border-white/50 text-white px-8 py-4 rounded-lg font-bold text-lg hover:bg-white/10 transition-all duration-300">
                    Partner With Us
                </a>
            </div>

            <!-- Key Stats -->
            <div class="grid grid-cols-2 md:grid-cols-4 gap-4 md:gap-6 max-w-4xl mx-auto">
                <div class="stat-card rounded-xl p-5">
                    <div class="text-3xl md:text-4xl font-bold">13,200+</div>
                    <div class="text-sm text-blue-200 mt-1">Target Seniors</div>
                </div>
                <div class="stat-card rounded-xl p-5">
                    <div class="text-3xl md:text-4xl font-bold">2</div>
                    <div class="text-sm text-blue-200 mt-1">Pilot Markets</div>
                </div>
                <div class="stat-card rounded-xl p-5">
                    <div class="text-3xl md:text-4xl font-bold">2</div>
                    <div class="text-sm text-blue-200 mt-1">States</div>
                </div>
                <div class="stat-card rounded-xl p-5">
                    <div class="text-3xl md:text-4xl font-bold">$30-60</div>
                    <div class="text-sm text-blue-200 mt-1">PMPM Revenue</div>
                </div>
            </div>
        </div>
    </section>
```

**Step 2: Delete the Dual-Pilot Advantage Banner (lines 398-407)**

Remove the entire gold strip banner section. It's absorbed into the For Investors section later.

**Step 3: Commit**

```bash
git add public/index.html
git commit -m "feat: rewrite hero with family-first messaging and language preservation mention"
```

---

### Task 3: Replace Solution with How It Works

**Files:**
- Modify: `public/index.html` — replace lines 409-505 (Solution section)

**Step 1: Replace the Solution section with How It Works**

```html
    <!-- How It Works -->
    <section id="how-it-works" class="py-20 px-4">
        <div class="max-w-6xl mx-auto">
            <div class="text-center mb-14">
                <span class="inline-block bg-brand-gold text-white text-xs font-bold px-3 py-1 rounded-full uppercase tracking-wide mb-3">Simple by Design</span>
                <h2 class="text-3xl md:text-4xl font-bold text-brand-blue mb-4 fade-up">
                    We Call. We Listen. We Alert.
                </h2>
                <p class="text-gray-600 max-w-2xl mx-auto text-lg fade-up fade-up-delay-1">
                    Three steps. No technology burden on your loved one.
                </p>
            </div>

            <div class="grid md:grid-cols-3 gap-8 max-w-5xl mx-auto">
                <!-- Step 1: We Call -->
                <div class="text-center fade-up fade-up-delay-1">
                    <div class="w-16 h-16 bg-blue-100 rounded-2xl flex items-center justify-center mx-auto mb-5">
                        <svg class="w-8 h-8 text-brand-blue" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 5a2 2 0 012-2h3.28a1 1 0 01.948.684l1.498 4.493a1 1 0 01-.502 1.21l-2.257 1.13a11.042 11.042 0 005.516 5.516l1.13-2.257a1 1 0 011.21-.502l4.493 1.498a1 1 0 01.684.949V19a2 2 0 01-2 2h-1C9.716 21 3 14.284 3 6V5z"/>
                        </svg>
                    </div>
                    <div class="text-brand-gold font-bold text-sm uppercase tracking-wide mb-2">Step 1</div>
                    <h3 class="text-xl font-bold text-gray-800 mb-3">We Call</h3>
                    <p class="text-gray-600 leading-relaxed">
                        Sunny calls your loved one every day at their preferred time. No app to install, no internet needed — just their regular phone.
                    </p>
                </div>

                <!-- Step 2: We Listen -->
                <div class="text-center fade-up fade-up-delay-2">
                    <div class="w-16 h-16 bg-blue-100 rounded-2xl flex items-center justify-center mx-auto mb-5">
                        <svg class="w-8 h-8 text-brand-blue" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 12h.01M12 12h.01M16 12h.01M21 12c0 4.418-4.03 8-9 8a9.863 9.863 0 01-4.255-.949L3 20l1.395-3.72C3.512 15.042 3 13.574 3 12c0-4.418 4.03-8 9-8s9 3.582 9 8z"/>
                        </svg>
                    </div>
                    <div class="text-brand-gold font-bold text-sm uppercase tracking-wide mb-2">Step 2</div>
                    <h3 class="text-xl font-bold text-gray-800 mb-3">We Listen</h3>
                    <p class="text-gray-600 leading-relaxed">
                        A warm, natural conversation — medication reminders, mood check-ins, home safety questions. AI that sounds like a friend, not a robot.
                    </p>
                </div>

                <!-- Step 3: We Alert -->
                <div class="text-center fade-up fade-up-delay-3">
                    <div class="w-16 h-16 bg-blue-100 rounded-2xl flex items-center justify-center mx-auto mb-5">
                        <svg class="w-8 h-8 text-brand-blue" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 17h5l-1.405-1.405A2.032 2.032 0 0118 14.158V11a6.002 6.002 0 00-4-5.659V5a2 2 0 10-4 0v.341C7.67 6.165 6 8.388 6 11v3.159c0 .538-.214 1.055-.595 1.436L4 17h5m6 0v1a3 3 0 11-6 0v-1m6 0H9"/>
                        </svg>
                    </div>
                    <div class="text-brand-gold font-bold text-sm uppercase tracking-wide mb-2">Step 3</div>
                    <h3 class="text-xl font-bold text-gray-800 mb-3">We Alert</h3>
                    <p class="text-gray-600 leading-relaxed">
                        If something's off — a missed call, a fall, a worrying pattern — family and caregivers are notified immediately.
                    </p>
                </div>
            </div>
        </div>
    </section>
```

**Step 2: Commit**

```bash
git add public/index.html
git commit -m "feat: replace Solution section with family-friendly How It Works"
```

---

### Task 4: Replace Platform Ready with Technology

**Files:**
- Modify: `public/index.html` — replace lines 584-628 (Platform Ready section, between Demos and old Dual-Pilot)

**Step 1: Replace Platform Ready with Technology section**

```html
    <!-- Technology -->
    <section id="technology" class="py-16 px-4 bg-gradient-to-r from-brand-blue to-brand-blue-light text-white">
        <div class="max-w-6xl mx-auto">
            <div class="text-center mb-10">
                <span class="inline-block bg-brand-gold text-white text-xs font-bold px-3 py-1 rounded-full uppercase tracking-wide mb-3">Our Technology</span>
                <h2 class="text-2xl md:text-3xl font-bold mb-3 fade-up">Enterprise-Grade AI. Any Phone.</h2>
                <p class="text-blue-200 max-w-3xl mx-auto fade-up fade-up-delay-1">
                    Same platform powers daily wellness calls in English and Nimipuut&iacute;mt language preservation with the Nez Perce Tribe.
                </p>
            </div>

            <div class="grid md:grid-cols-3 gap-6 max-w-4xl mx-auto mb-10">
                <div class="bg-white/10 backdrop-blur-sm rounded-xl p-6 text-center border border-white/10 fade-up fade-up-delay-1">
                    <div class="w-12 h-12 bg-white/20 rounded-lg flex items-center justify-center mx-auto mb-3">
                        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 5a2 2 0 012-2h3.28a1 1 0 01.948.684l1.498 4.493a1 1 0 01-.502 1.21l-2.257 1.13a11.042 11.042 0 005.516 5.516l1.13-2.257a1 1 0 011.21-.502l4.493 1.498a1 1 0 01.684.949V19a2 2 0 01-2 2h-1C9.716 21 3 14.284 3 6V5z"/>
                        </svg>
                    </div>
                    <h3 class="font-bold text-lg mb-1">Amazon Connect</h3>
                    <p class="text-blue-200 text-sm">Telephony &amp; Voice</p>
                </div>
                <div class="bg-white/10 backdrop-blur-sm rounded-xl p-6 text-center border border-white/10 fade-up fade-up-delay-2">
                    <div class="w-12 h-12 bg-white/20 rounded-lg flex items-center justify-center mx-auto mb-3">
                        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19.114 5.636a9 9 0 010 12.728M16.463 8.288a5.25 5.25 0 010 7.424M6.75 8.25l4.72-4.72a.75.75 0 011.28.53v15.88a.75.75 0 01-1.28.53l-4.72-4.72H4.51c-.88 0-1.704-.507-1.938-1.354A9.01 9.01 0 012.25 12c0-.83.112-1.633.322-2.396C2.806 8.756 3.63 8.25 4.51 8.25H6.75z"/>
                        </svg>
                    </div>
                    <h3 class="font-bold text-lg mb-1">NVIDIA Riva</h3>
                    <p class="text-blue-200 text-sm">Speech AI (ASR/TTS)</p>
                </div>
                <div class="bg-white/10 backdrop-blur-sm rounded-xl p-6 text-center border border-white/10 fade-up fade-up-delay-3">
                    <div class="w-12 h-12 bg-white/20 rounded-lg flex items-center justify-center mx-auto mb-3">
                        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9.75 3.104v5.714a2.25 2.25 0 01-.659 1.591L5 14.5M9.75 3.104c-.251.023-.501.05-.75.082m.75-.082a24.301 24.301 0 014.5 0m0 0v5.714a2.25 2.25 0 00.659 1.591L19 14.5M14.25 3.104c.251.023.501.05.75.082M19 14.5l-2.47 2.47a2.25 2.25 0 01-1.59.659H9.06a2.25 2.25 0 01-1.591-.659L5 14.5m14 0V19a2 2 0 01-2 2H7a2 2 0 01-2-2v-4.5"/>
                        </svg>
                    </div>
                    <h3 class="font-bold text-lg mb-1">Claude via Bedrock</h3>
                    <p class="text-blue-200 text-sm">Conversational AI</p>
                </div>
            </div>

            <div class="flex flex-wrap items-center justify-center gap-x-6 gap-y-3 text-sm text-blue-200">
                <span class="flex items-center gap-2">
                    <svg class="w-4 h-4 text-brand-gold" fill="currentColor" viewBox="0 0 20 20"><path fill-rule="evenodd" d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z" clip-rule="evenodd"/></svg>
                    No app required
                </span>
                <span class="flex items-center gap-2">
                    <svg class="w-4 h-4 text-brand-gold" fill="currentColor" viewBox="0 0 20 20"><path fill-rule="evenodd" d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z" clip-rule="evenodd"/></svg>
                    Works on any phone
                </span>
                <span class="flex items-center gap-2">
                    <svg class="w-4 h-4 text-brand-gold" fill="currentColor" viewBox="0 0 20 20"><path fill-rule="evenodd" d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z" clip-rule="evenodd"/></svg>
                    HIPAA compliance path (BAA in progress)
                </span>
                <a href="https://www.nvidia.com/en-us/startups/" target="_blank" rel="noopener noreferrer" class="inline-flex items-center gap-2 hover:opacity-80 transition-opacity">
                    <img src="./assets/nvidia-inception-badge.png" alt="NVIDIA Inception Program Member" class="h-8 w-auto">
                </a>
            </div>
        </div>
    </section>
```

**Step 2: Commit**

```bash
git add public/index.html
git commit -m "feat: replace Platform Ready with Technology section including language preservation"
```

---

### Task 5: Delete Old Middle Sections, Add For Partners + For Investors

**Files:**
- Modify: `public/index.html` — delete lines for Dual-Pilot Details (629-746), Business Model (748-939), and old Partners (941-1004). Replace with For Partners + For Investors.

**Step 1: Delete the Dual-Pilot Details section, Business Model section, and old Partners section. Replace with:**

```html
    <!-- For Partners -->
    <section id="partners" class="bg-gray-50 py-20 px-4">
        <div class="max-w-6xl mx-auto">
            <div class="text-center mb-14">
                <span class="inline-block bg-brand-blue text-white text-xs font-bold px-3 py-1 rounded-full uppercase tracking-wide mb-3">For Partners</span>
                <h2 class="text-3xl md:text-4xl font-bold text-brand-blue mb-4 fade-up">
                    Integrate Sunny Into Your Care Network
                </h2>
                <p class="text-gray-600 max-w-2xl mx-auto text-lg fade-up fade-up-delay-1">
                    Free 3-month pilot. No integration cost. Measurable outcomes.
                </p>
            </div>

            <div class="grid md:grid-cols-2 gap-6 lg:gap-8">
                <!-- AAA Integration -->
                <div class="bg-white rounded-2xl shadow-lg p-7 border-l-4 border-brand-blue card-hover fade-up fade-up-delay-1">
                    <div class="w-12 h-12 bg-blue-100 rounded-xl flex items-center justify-center mb-5">
                        <svg class="w-6 h-6 text-brand-blue" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 21V5a2 2 0 00-2-2H7a2 2 0 00-2 2v16m14 0h2m-2 0h-5m-9 0H3m2 0h5M9 7h1m-1 4h1m4-4h1m-1 4h1m-5 10v-5a1 1 0 011-1h2a1 1 0 011 1v5m-4 0h4"/>
                        </svg>
                    </div>
                    <h3 class="text-xl font-bold text-gray-800 mb-3">Area Agencies on Aging</h3>
                    <p class="text-gray-600 mb-4">
                        Plug into your existing OAA-funded services. Sunny handles daily check-ins, your case managers get actionable alerts. 622 AAAs nationwide — we start with Region II and Region IV.
                    </p>
                    <p class="text-xs text-gray-400">Integration: OAA Title III services, Medicaid HCBS waiver programs</p>
                </div>

                <!-- Tribal Health -->
                <div class="bg-white rounded-2xl shadow-lg p-7 border-l-4 border-brand-gold card-hover fade-up fade-up-delay-2">
                    <div class="w-12 h-12 bg-yellow-100 rounded-xl flex items-center justify-center mb-5">
                        <svg class="w-6 h-6 text-brand-gold" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4.318 6.318a4.5 4.5 0 000 6.364L12 20.364l7.682-7.682a4.5 4.5 0 00-6.364-6.364L12 7.636l-1.318-1.318a4.5 4.5 0 00-6.364 0z"/>
                        </svg>
                    </div>
                    <h3 class="text-xl font-bold text-gray-800 mb-3">Nimiipuu Health / Nez Perce Tribe</h3>
                    <p class="text-gray-600 mb-4">
                        Culturally responsive wellness calls for tribal elders. Dual-purpose: health monitoring + Nimipuut&iacute;mt language preservation. Proof-of-concept for IHS-funded tribal health programs nationwide.
                    </p>
                    <p class="text-xs text-gray-400">Integration: Indian Health Service, tribal elder care programs</p>
                </div>

                <!-- Health Systems -->
                <div class="bg-white rounded-2xl shadow-lg p-7 border-l-4 border-brand-blue card-hover fade-up fade-up-delay-1">
                    <div class="w-12 h-12 bg-blue-100 rounded-xl flex items-center justify-center mb-5">
                        <svg class="w-6 h-6 text-brand-blue" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4.26 10.147a60.436 60.436 0 00-.491 6.347A48.627 48.627 0 0112 20.904a48.627 48.627 0 018.232-4.41 60.46 60.46 0 00-.491-6.347m-15.482 0a50.57 50.57 0 00-2.658-.813A59.905 59.905 0 0112 3.493a59.902 59.902 0 0110.399 5.84c-.896.248-1.783.52-2.658.814m-15.482 0A50.697 50.697 0 0112 13.489a50.702 50.702 0 017.74-3.342"/>
                        </svg>
                    </div>
                    <h3 class="text-xl font-bold text-gray-800 mb-3">Regional Hospitals &amp; ACOs</h3>
                    <p class="text-gray-600 mb-4">
                        Reduce readmissions and ER utilization. Integrates with your care coordination workflows. Current targets: Tri-State Memorial, St. Luke's Wood River, St. Joseph Regional Medical Center.
                    </p>
                    <p class="text-xs text-gray-400">Integration: care coordination, discharge follow-up, population health</p>
                </div>

                <!-- Device Integrations -->
                <div class="bg-white rounded-2xl shadow-lg p-7 border-l-4 border-brand-green card-hover fade-up fade-up-delay-2">
                    <div class="w-12 h-12 bg-green-100 rounded-xl flex items-center justify-center mb-5">
                        <svg class="w-6 h-6 text-brand-green" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 3v2m6-2v2M9 19v2m6-2v2M5 9H3m2 6H3m18-6h-2m2 6h-2M7 19h10a2 2 0 002-2V7a2 2 0 00-2-2H7a2 2 0 00-2 2v10a2 2 0 002 2zM9 9h6v6H9V9z"/>
                        </svg>
                    </div>
                    <h3 class="text-xl font-bold text-gray-800 mb-3">Connected Health Devices</h3>
                    <p class="text-gray-600 mb-4">
                        Sunny interprets data from Dexcom G7 CGM and CPAP/ResMed monitors during daily calls. "I see your glucose was 180 last night — let's talk about that." Turns passive data into active conversations.
                    </p>
                    <p class="text-xs text-gray-400">Devices: Dexcom G7, ResMed AirSense, future: blood pressure, pulse ox</p>
                </div>
            </div>
        </div>
    </section>

    <!-- For Investors -->
    <section id="investors" class="py-20 px-4">
        <div class="max-w-6xl mx-auto">
            <div class="text-center mb-14">
                <span class="inline-block bg-brand-green text-white text-xs font-bold px-3 py-1 rounded-full uppercase tracking-wide mb-3">For Investors</span>
                <h2 class="text-3xl md:text-4xl font-bold text-brand-blue mb-4 fade-up">
                    The Aging-in-Place Market Is Massive and Underserved
                </h2>
            </div>

            <div class="grid md:grid-cols-2 gap-8 lg:gap-12">
                <!-- Key Metrics -->
                <div class="space-y-4 fade-up fade-up-delay-1">
                    <div class="bg-gradient-to-r from-blue-50 to-blue-100 rounded-xl p-6 flex items-center gap-5">
                        <div class="text-3xl md:text-4xl font-extrabold text-brand-blue">$1M</div>
                        <div>
                            <div class="font-bold text-gray-800">Seed Round</div>
                            <div class="text-sm text-gray-500">Raising now</div>
                        </div>
                    </div>
                    <div class="bg-gradient-to-r from-blue-50 to-blue-100 rounded-xl p-6 flex items-center gap-5">
                        <div class="text-3xl md:text-4xl font-extrabold text-brand-blue">55M+</div>
                        <div>
                            <div class="font-bold text-gray-800">Americans 65+</div>
                            <div class="text-sm text-gray-500">73M by 2030</div>
                        </div>
                    </div>
                    <div class="bg-gradient-to-r from-blue-50 to-blue-100 rounded-xl p-6 flex items-center gap-5">
                        <div class="text-3xl md:text-4xl font-extrabold text-brand-blue">622</div>
                        <div>
                            <div class="font-bold text-gray-800">Area Agencies on Aging</div>
                            <div class="text-sm text-gray-500">National distribution channel</div>
                        </div>
                    </div>
                    <div class="bg-gradient-to-r from-green-50 to-green-100 rounded-xl p-6 flex items-center gap-5">
                        <div class="text-2xl md:text-3xl font-extrabold text-brand-green">Series A</div>
                        <div>
                            <div class="font-bold text-gray-800">Planned Post-Pilot</div>
                            <div class="text-sm text-gray-500">Month 18-24</div>
                        </div>
                    </div>
                </div>

                <!-- Strategy -->
                <div class="fade-up fade-up-delay-2">
                    <div class="bg-white rounded-2xl shadow-lg p-8">
                        <h3 class="text-xl font-bold text-gray-800 mb-4">Dual-Pilot Validation Strategy</h3>
                        <p class="text-gray-600 mb-6 leading-relaxed">
                            We're validating across economic segments before scaling nationally.
                            <strong>Lewis-Clark Valley</strong> tests the Medicaid/rural model.
                            <strong>Wood River Valley</strong> tests private-pay/affluent.
                            If it works across both, it works anywhere in America.
                        </p>
                        <ul class="space-y-3 text-gray-600">
                            <li class="flex items-start gap-3">
                                <svg class="w-5 h-5 text-brand-gold mt-0.5 flex-shrink-0" fill="currentColor" viewBox="0 0 20 20"><path fill-rule="evenodd" d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z" clip-rule="evenodd"/></svg>
                                <span>NVIDIA Inception member</span>
                            </li>
                            <li class="flex items-start gap-3">
                                <svg class="w-5 h-5 text-brand-gold mt-0.5 flex-shrink-0" fill="currentColor" viewBox="0 0 20 20"><path fill-rule="evenodd" d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z" clip-rule="evenodd"/></svg>
                                <span>Amazon Connect + NVIDIA Riva + Claude/Bedrock stack</span>
                            </li>
                            <li class="flex items-start gap-3">
                                <svg class="w-5 h-5 text-brand-gold mt-0.5 flex-shrink-0" fill="currentColor" viewBox="0 0 20 20"><path fill-rule="evenodd" d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z" clip-rule="evenodd"/></svg>
                                <span>HIPAA compliance path — BAA in progress</span>
                            </li>
                            <li class="flex items-start gap-3">
                                <svg class="w-5 h-5 text-brand-gold mt-0.5 flex-shrink-0" fill="currentColor" viewBox="0 0 20 20"><path fill-rule="evenodd" d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z" clip-rule="evenodd"/></svg>
                                <span>Grant-funded Year 1, revenue Month 12</span>
                            </li>
                        </ul>
                        <div class="mt-6 pt-6 border-t border-gray-100">
                            <a href="mailto:info@safe4seniors.ai?subject=Investment Deck Request"
                               class="btn-primary inline-block bg-brand-blue text-white px-6 py-3 rounded-lg font-bold hover:bg-brand-blue-light">
                                Request Investment Deck
                            </a>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
```

**Step 2: Commit**

```bash
git add public/index.html
git commit -m "feat: replace Business Model/Dual-Pilot/Partners with For Partners + For Investors sections"
```

---

### Task 6: Add Dual Mission Section

**Files:**
- Modify: `public/index.html` — insert new section between For Investors and Team

**Step 1: Insert the Dual Mission section after the For Investors closing `</section>` and before the Team `<!-- Team Section -->` comment:**

```html
    <!-- Dual Mission -->
    <section id="mission" class="py-20 px-4 bg-gradient-to-br from-blue-50 via-white to-green-50">
        <div class="max-w-6xl mx-auto">
            <div class="text-center mb-14">
                <span class="inline-block bg-brand-gold text-white text-xs font-bold px-3 py-1 rounded-full uppercase tracking-wide mb-3">Our Mission</span>
                <h2 class="text-3xl md:text-4xl font-bold text-brand-blue mb-4 fade-up">
                    Two Missions. One Platform.
                </h2>
            </div>

            <div class="grid md:grid-cols-2 gap-8 max-w-5xl mx-auto">
                <!-- Primary Mission -->
                <div class="bg-white rounded-2xl shadow-lg p-8 border-t-4 border-brand-blue card-hover fade-up fade-up-delay-1">
                    <div class="w-14 h-14 bg-blue-100 rounded-xl flex items-center justify-center mb-5">
                        <svg class="w-7 h-7 text-brand-blue" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4.318 6.318a4.5 4.5 0 000 6.364L12 20.364l7.682-7.682a4.5 4.5 0 00-6.364-6.364L12 7.636l-1.318-1.318a4.5 4.5 0 00-6.364 0z"/>
                        </svg>
                    </div>
                    <h3 class="text-xl font-bold text-gray-800 mb-3">Healthcare AI for Aging in Place</h3>
                    <p class="text-gray-600 leading-relaxed">
                        Daily wellness calls that catch problems before they become crises. Keeping seniors independent, safe, and connected to the people who love them.
                    </p>
                </div>

                <!-- Secondary Mission -->
                <div class="bg-white rounded-2xl shadow-lg p-8 border-t-4 border-brand-gold card-hover fade-up fade-up-delay-2">
                    <div class="w-14 h-14 bg-yellow-100 rounded-xl flex items-center justify-center mb-5">
                        <svg class="w-7 h-7 text-brand-gold" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 12h.01M12 12h.01M16 12h.01M21 12c0 4.418-4.03 8-9 8a9.863 9.863 0 01-4.255-.949L3 20l1.395-3.72C3.512 15.042 3 13.574 3 12c0-4.418 4.03-8 9-8s9 3.582 9 8z"/>
                        </svg>
                    </div>
                    <h3 class="text-xl font-bold text-gray-800 mb-3">Nimipuut&iacute;mt Language Preservation</h3>
                    <p class="text-gray-600 leading-relaxed">
                        The same AI platform that conducts wellness calls can hold conversations in endangered languages. Our partnership with the Nez Perce Tribe isn't just healthcare — it's cultural survival. Fewer than 100 fluent Nimipuut&iacute;mt speakers remain.
                    </p>
                </div>
            </div>

            <div class="text-center mt-10 max-w-3xl mx-auto fade-up">
                <blockquote class="text-gray-700 italic text-lg leading-relaxed">
                    "The Nez Perce Tribe is our proof-of-concept partner — if our platform can serve tribal elders in their own language, it can serve any community in America."
                </blockquote>
            </div>
        </div>
    </section>
```

**Step 2: Commit**

```bash
git add public/index.html
git commit -m "feat: add Dual Mission section — healthcare AI + language preservation"
```

---

### Task 7: Update Team Section

**Files:**
- Modify: `public/index.html` — update Team section content

**Step 1: Update Jay's card:**

- Title: "Community & Operations" -> "Co-Founder / CEO"
- Bio: Update to "Cross-disciplinary: AI/ML engineering, construction project management, GIS. 15+ years serving Idaho-Washington communities. Ties to Wood River Valley since 1997. Established relationships in both pilot markets."

**Step 2: Update Richard's card:**

- Title: "Chief Technology Officer" -> "Co-Founder / CHO"
- Bio: Update to "PhD Oxford. PATH, IntraHealth International, UNICEF — 15+ years global health across 20+ countries. Health IT standards (FHIR, HIPAA). DC federal healthcare connections (CMS, ACL, IHS)."

**Step 3: Update the "Why This Team Works" callout to reference new titles (CEO + CHO)**

**Step 4: Commit**

```bash
git add public/index.html
git commit -m "feat: update team titles — Jay as CEO, Richard as CHO"
```

---

### Task 8: Rewrite Contact/CTA Section

**Files:**
- Modify: `public/index.html` — replace Contact section

**Step 1: Replace the Contact section with:**

```html
    <!-- Contact Section -->
    <section id="contact" class="gradient-cta text-white py-20 px-4 relative overflow-hidden">
        <!-- Background decoration -->
        <div class="absolute inset-0 opacity-10">
            <div class="absolute top-0 right-0 w-96 h-96 rounded-full bg-white blur-3xl"></div>
            <div class="absolute bottom-0 left-0 w-64 h-64 rounded-full bg-white blur-3xl"></div>
        </div>

        <div class="max-w-4xl mx-auto text-center relative z-10">
            <h2 class="text-3xl md:text-4xl font-bold mb-4 fade-up">Let's Talk</h2>
            <p class="text-blue-200 mb-10 text-lg fade-up fade-up-delay-1">
                Whether you're exploring care for a loved one or looking to partner — we'd love to hear from you.
            </p>

            <div class="grid md:grid-cols-2 gap-6">
                <div class="bg-white/10 backdrop-blur-sm rounded-2xl p-7 border border-white/20 fade-up fade-up-delay-1">
                    <h3 class="text-xl font-bold mb-3">Schedule a Demo</h3>
                    <p class="text-blue-200 mb-5">See Sunny in action. 30-minute walkthrough for partners and investors.</p>
                    <a href="mailto:info@safe4seniors.ai?subject=Demo Request"
                       class="btn-primary inline-block bg-white text-brand-blue px-7 py-3.5 rounded-lg font-bold hover:bg-blue-50">
                        Book a Demo
                    </a>
                </div>
                <div class="bg-white/10 backdrop-blur-sm rounded-2xl p-7 border border-white/20 fade-up fade-up-delay-2">
                    <h3 class="text-xl font-bold mb-3">Learn More for Your Family</h3>
                    <p class="text-blue-200 mb-5">Have questions about how Sunny can help your loved one? We'll walk you through it.</p>
                    <a href="mailto:info@safe4seniors.ai?subject=Family Inquiry"
                       class="btn-primary inline-block bg-brand-gold text-white px-7 py-3.5 rounded-lg font-bold hover:bg-brand-gold-dark">
                        Get in Touch
                    </a>
                </div>
            </div>
        </div>
    </section>
```

**Step 2: Commit**

```bash
git add public/index.html
git commit -m "feat: rewrite Contact section with dual CTAs for partners and families"
```

---

### Task 9: Update Footer and Meta Tags

**Files:**
- Modify: `public/index.html` — footer nav links and head meta

**Step 1: Update footer nav links to:**

```html
<a href="#how-it-works" class="hover:text-white transition">How It Works</a>
<a href="#partners" class="hover:text-white transition">Partners</a>
<a href="#investors" class="hover:text-white transition">Investors</a>
<a href="#contact" class="hover:text-white transition">Contact</a>
```

**Step 2: Update meta description (line 7) to:**

```
"Your loved one. Checked on. Every day. AI-powered daily wellness calls for seniors aging in place — no app, no internet required. Also preserving the Nimipuutimt language with the Nez Perce Tribe."
```

**Step 3: Update OG description (line 16) and Twitter description (line 25) similarly.**

**Step 4: Commit**

```bash
git add public/index.html
git commit -m "feat: update footer nav links and meta descriptions"
```

---

### Task 10: Sync and Final Verification

**Files:**
- Copy: `public/index.html` -> `marketing/index.html`

**Step 1: Sync files**

```bash
cp public/index.html marketing/index.html
```

**Step 2: Verify no broken anchor links**

```bash
grep -oP 'href="#[^"]*"' public/index.html | sort -u
grep -oP 'id="[^"]*"' public/index.html | sort -u
```

Cross-check: every `href="#X"` has a matching `id="X"`.

**Step 3: Verify line count is reasonable**

```bash
wc -l public/index.html
```

Expected: ~900-1000 lines (down from 1281 — we removed ~400 lines of tables/projections and added ~200 lines of new content).

**Step 4: Commit**

```bash
git add public/index.html marketing/index.html
git commit -m "chore: sync marketing/ with public/, verify no broken links"
```
