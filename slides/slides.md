---
theme: default
title: Stop Running Mystery Meat in Production
info: |
  Stop Running Mystery Meat in Production.
  Jeroen van Erp, SUSE.
author: Jeroen van Erp
canvasWidth: 1280
aspectRatio: 16/9
colorSchema: light
transition: fade
mdc: true
drawings:
  persist: false
imageTheme: burgers
layout: photo
image: title.jpg
position: 0
bgzoom: 112
---

<div style="position:absolute; inset:0; background:linear-gradient(90deg, rgba(12,50,44,.9) 0%, rgba(12,50,44,.65) 38%, rgba(12,50,44,0) 68%);"></div>

<div style="position:absolute; left:6.2%; top:52%; transform:translateY(-50%); width:50%; text-shadow:0 2px 14px rgba(0,0,0,.55);">

<h1 style="font-size:5.6rem; font-weight: 800; line-height:1.33; margin:0;">Stop Running<br><span class="accent-orange">Mystery Meat</span><br>in Production</h1>

</div>

<!--
Let's talk about your lunch! Do you remember the last time you went to that fast food place around the corner of your office, and you decided to order the burger?

What did you get on your plate? Something maybe that was a tad too greasy, looking overcooked… You ordered the burger, but you know deep down... it's Mystery Meat. You don't know where it came from. You don't know what's inside it. Did they actually make this from an animal? But you eat it anyway because it's cheap and it's there.

Well, I have bad news. You are doing the exact same thing with your production infrastructure. Today, we're going to talk about why we keep running 'Mystery Meat' containers, and how to stop doing that!
-->

---
layout: intro
---

<img src="/img/speaker.png" style="position:absolute; left:0; bottom:0; height:100%; z-index:6;" alt="Jeroen van Erp with a sheep" />

<div style="position:absolute; left:43%; top:6.2%; right:3rem;">

# Jeroen van Erp

<p style="margin:-0.6rem 0 0.2rem; opacity:1;"><b class="accent-green">SUSE</b> | <span class="accent-green">Principal Technology Advocate</span></p>
<p style="font-style:italic; font-size:1.55rem; opacity:.8; margin:0 0 1.4rem;">“Connecting platform engineering to business outcomes”</p>

- 25 years across Engineering, Operations and Product
- 👦🏼👧🏼👦🏼👦🏼 + 🐑🐑🐑🐑 → complexity doesn't stop after work

</div>

<!--
But before we start dissecting our mystery meat, let's do some introductions!

I'm Jeroen van Erp. I've been doing this for 25 years, from Engineering to Operations to Product.

Currently, I'm a Principal Technology Advocate at SUSE. Which means my job is simple: I try to connect the geeky reality of Platform Engineering to actual Business Outcomes.

So right now, the biggest threat to those business outcomes is the complexity we are dragging into our systems.
-->

---
layout: photo
image: pop-quiz.jpg
position: 70
clicks: 2
---

<h2 class="ptitle" style="font-size:3.1rem;">Pop Quiz</h2>

<div class="fx" :class="{ on: $clicks === 1 }" style="position:absolute; left:18%; top:45%; width:64%;">
  <Terminal title="Dockerfile" icon="🐳" :lines="['FROM node:latest']" style="--term-s:2; --term-bp:1.65rem; --term-size:2.45rem; --term-pad:2.3rem 2.4rem 2rem 1.25rem; --term-gap:3.2rem; --term-nw:1ch;" />
</div>

<div class="fx" :class="{ on: $clicks >= 2 }" style="position:absolute; left:13.4%; top:40%; width:73.4%;">

<Terminal icon="›_" style="--term-s:1.6; --term-bp:1.4rem; --term-lh:1.66; --term-pad:2rem 2rem 1.2rem;">
<pre class="grype"><span class="t-prompt">❯</span> grype node:latest
 <span class="ok">✔</span> <span class="o">Vulnerability DB</span>                <span class="d">[updated]</span>
 <span class="ok">✔</span> <span class="o">Parsed</span> image
 <span class="ok">✔</span> <span class="o">Cataloged</span> contents
   ├── <span class="ok">✔</span> <span class="o">Packages</span>                        <span class="d">[</span><span class="n">600</span> packages<span class="d">]</span>
   ├── <span class="ok">✔</span> <span class="o">Executables</span>                     <span class="d">[</span><span class="n">1,312</span> executables<span class="d">]</span>
   ├── <span class="ok">✔</span> <span class="o">File</span> metadata                   <span class="d">[</span><span class="n">21,163</span> locations<span class="d">]</span>
   └── <span class="ok">✔</span> <span class="o">File</span> digests                    <span class="d">[</span><span class="n">21,163</span> files<span class="d">]</span>
 <span class="ok">✔</span> <span class="o">Scanned for</span> vulnerabilities     <span class="d">[</span><span class="n">2041</span> vulnerability matches<span class="d">]</span>
   └── <span class="d">by severity:</span> <span class="n">184</span> critical, <span class="n">771</span> high, <span class="n">614</span> medium, <span class="n">149</span> low, <span class="n">933</span> <span class="p">negligible</span> <span class="d">(</span><span class="n">470</span> unknown<span class="d">)</span></pre>
</Terminal>

</div>

<span class="footnote">* as of 2026-08-24</span>

<!--
Let's start with a quick pop-quiz: "How many lines of code does it take to deliver more than 2000 vulnerabilities into production?" Does anyone want to venture a guess?

<wait>

Well, it's not as much as you may think! We only need 1 line of code! If we pull in the latest NodeJS image from Docker Hub we get all of those vulnerabilities for free!

If you don't believe me, here's the scan I did last month!
-->

---
layout: photo
transition: view-transition
---

<Iceberg :step="1" />

<!--
Why is this? When you ship an application, you of course write a ton of business logic, "Your Application Code". This is what you're paid for as a developer.

That code of course relies on a bunch of libraries that provide functionality such as an HTTP Server, maybe a Rest or GraphQL framework, libraries to interface with a database, etc. All of these are nicely documented in your package.json.

And then you put all of that in a box called nodejs:latest and ship that off to production.
-->

---
layout: photo
clicks: 2
transition: view-transition
---

<Iceberg :step="2" :click="$clicks" />

<!--
However what happened when you put it in that box? Well, of course you got the language runtime, your Node interpreter.

But next to that, you also got all of the required Linux Libraries for free, and what's more, that box contains a full Linux Kernel.

So in essence, you're shipping an entire Linux OS together with your application.

Your application code, the stuff you've really written, is maybe at most 1% of the size of what you're shipping towards your production system. The other 99% is the mystery meat!
-->

---
layout: photo
---

<Iceberg :step="3" />

<!--
And that 99% came with the first line of that Docker container that you delivered. The "FROM node:latest".

When you do this, what you're essentially doing is trusting someone on the internet to build your entire foundation for your application.

Who built it? Unknown!
Where did they build it? Maybe on their laptop, was it even reproducible?
Did they add any backdoors or malicious binaries? Unknown.

This is where you're compromising your entire supply chain!
-->

---
layout: statement
---

<div style="text-align:left;">
  <div style="font-size:4rem; line-height:1.15;">“I just need to ship my code…”</div>
  <p style="font-style:italic; font-size:3.4rem; margin:1rem 0 0;">– Any developer anywhere</p>
</div>

<!--
Now, if you go to a developer and say: 'Hey, you need to audit these 19,000 files in the base OS,' what do they say?

'Stop blocking me. I just need to ship my code.'

And they are right to some extent! It is not their job to be Linux kernel maintainers.
-->

---
layout: photo
image: developer.jpg
panel: right
size: sm
---

<h1 style="font-size:2.6rem;">The Developer Perspective</h1>

- Deliver Features in 2 week sprints
- It works on my machine
- Base image is a Utility

<!--
Let's look at this through the eyes of your developers.

This is their world. It feels like a high paced counter at a fast food joint. They get tickets (orders) in and need to deliver them as fast as possible to the stakeholder (customer).

Their incentives are clear: Move tickets from 'To Do' to 'Done.' In 2 week increments. End of the sprint, they will have tested the features and packaged them up. It worked on their machines.

They live for Velocity. They want to grab a library, build the feature, and ship it.

To a developer, a container isn't an operating system; it's just a packaging format. It's the box the burger comes in. They don't want to know how the cardboard was manufactured; they just want to deliver the order to the customer.
-->

---
layout: photo
image: platform-engineer.jpg
panel: left
size: sm
---

<h1 style="font-size:3.1rem;">The Platform Engineer’s Perspective</h1>

- An entire Debian distribution in a box
- Tons of attack vectors (shell, compilers)
- `runAsRoot=true`
- Who will patch the vulnerabilities?

<!--
Now, look at the world through the eyes of the Platform Engineer. You are the Health Inspector, or the Hazmat Team.

You don't care about how fast that burger was delivered; you see a box which is leaking radioactive fluid and smelling badly.

When you see FROM node:latest, you don't see 'efficiency.' You see 'Unknown Ingredients.' What you're getting from those developers is unpatched CVEs, potential malware, and license violations. Worst of all, it's running as root and has a potential to harm your entire production system.

Your main concern is how to make this safe for consumption.
-->

---
layout: photo
image: trim-fat.jpg
---

<h1 class="ptitle" style="top:3.6rem;">Trimming the Fat</h1>

<div style="position:absolute; left:56.3%; top:8.5%; width:33%;">
<Terminal title="Dockerfile" icon="🐳" :lines="['FROM node:26-slim']" />

<div class="tight" style="margin-top:1.1rem; padding-left:.75rem; font-size:1.55rem;">

- “only” 128 vulnerabilities
- Smaller attack surface

</div>
</div>

---
layout: photo
image: trim-mystery.jpg
---

<h1 class="ptitle" style="top:3.6rem;">Trimming the Fat, not the Mystery</h1>

<div style="position:absolute; left:56.3%; top:8.5%; width:33%;">
<Terminal title="Dockerfile" icon="🐳" :lines="['FROM node:26-slim']" />

<div class="tight" style="margin-top:1.1rem; padding-left:.75rem; font-size:1.55rem;">

- “only” 128 vulnerabilities
- Smaller attack surface
- But it's still Mystery Meat

</div>
</div>

---
layout: photo
image: speed-trust.jpg
---

<div style="position:absolute; inset:0; background:rgba(12,50,44,.72);"></div>

<div style="position:absolute; left:0; right:0; top:26%; transform:translateY(-50%); text-align:center; font-size:2.65rem;" class="accent-green">Developers want</div>
<div style="position:absolute; left:0; right:0; top:35.5%; transform:translateY(-50%); text-align:center; font-size:4.25rem; font-weight:500;">Speed</div>
<div style="position:absolute; left:0; right:0; top:57%; transform:translateY(-50%); text-align:center; font-size:2.65rem;" class="accent-green">Platform Engineers want</div>
<div style="position:absolute; left:0; right:0; top:66.5%; transform:translateY(-50%); text-align:center; font-size:4.25rem; font-weight:500;">Trust</div>

<!--
So, we have a fundamental misalignment. Developers are rewarded for shipping features in 2-week sprints, leading them to the 'Wild West'. Platform Engineers are rewarded for stability and trust.

To prevent a stalemate, the compromise usually is: 'Okay, pull what you want, but we will scan it before it goes to production.'
-->

---
layout: photo
image: scan-chart-bg.jpg
clicks: 2
---

<div class="fx" :class="{ on: $clicks >= 1 }" style="position:absolute; left:50%; top:3%; transform:translateX(-50%); width:81%;">
  <ScanChart />
</div>

<h1 style="position:absolute; left:0; right:0; top:80%; text-align:center; font-size:4.7rem; margin:0; text-shadow:0 2px 10px rgba(0,0,0,.7);"><Typewriter text="You can’t scan yourself to safety" :active="$clicks >= 2" /></h1>

<!--
And that compromise is a trap. Because when you try to scan your way out of the 'Wild West,' you end up on the Hamster Wheel of Patching. You get the vulnerability report and start patching, end of the week you've finally tackled all of the vulnerabilities. You commit and ship it, and lo-and-behold, next week Monday new vulnerabilities have been found and you can start over again. You're reactive, you're always behind, and you're patching things you didn't even write.

And the pace at which vulnerabilities are being reported is increasing. In the chart, we see the first half year numbers of the past year vs the EOY numbers. We see that in H1 of 2026 already more vulnerabilities were reported than in the full year of 2023. The expected number of vulnerabilities reported will be around 72,000 this year.

So, you can't scan yourself to safety. We need to tackle our problem in a different way.
-->

---
layout: photo
image: scan-chart-bg.jpg
---

<style>
.upkeep .fg { position:absolute; }
.upkeep .track { left:6.7%; top:65.5%; width:59.3%; height:5.2%; border-radius:10px; background:rgba(255,255,255,.14); overflow:hidden; }
.upkeep .track i { display:block; width:25%; height:100%; background:var(--persimmon); }
.upkeep .logo { left:7.8%; bottom:3.2%; display:flex; align-items:center; gap:.4rem; }
.upkeep .logo img { height:1.1rem; }
.upkeep .logo span { font-weight:500; font-size:1.15rem; letter-spacing:.08em; }
</style>

<div class="upkeep">
  <div style="position:absolute; inset:0; background:rgba(12,50,44,.84);"></div>

  <div class="fg accent-green" style="left:7%; top:12.5%; font-size:13.5rem; font-weight:400; line-height:1; letter-spacing:-.02em;">84%</div>
  <div class="fg" style="left:6.7%; top:43.5%; width:60%; font-size:2.1rem; line-height:1.3;">of IT leaders admit that at least a quarter of their engineering capacity is consumed by routine platform upkeep</div>
  <div class="fg track"><i></i></div>
  <div class="fg" style="left:6.7%; top:72.5%; font-size:1.5rem; color:var(--mint);">At least a quarter: patching, upgrades, compliance evidence</div>

  <a class="fg" href="https://www.suse.com/report/hybrid-cloud-survey-2026/" target="_blank" rel="noopener noreferrer" aria-label="View the hybrid cloud survey results" style="left:73.5%; top:11.9%; width:19.8%; color:#fff; text-decoration:none; border:0;">
    <div style="box-sizing:border-box; width:100%; aspect-ratio:1; padding:3%; border-radius:12px; background:#fff;">
    <img src="/img/qr-survey-results.png" style="display:block; width:100%; height:100%; object-fit:contain;" alt="QR code linking to the hybrid cloud survey results" />
    </div>
    <div style="margin-top:1.2rem; font-size:1.5rem; font-weight:500;">Full survey results</div>
    <div style="margin-top:.5rem; font-size:1.1rem; line-height:1.35; color:var(--mint);">suse.com/report/hybrid-cloud-survey-2026</div>
  </a>

  <div class="fg" style="right:6.7%; bottom:3.6%; white-space:nowrap; text-align:right; font-size:.9rem; font-style:italic; line-height:1.5; color:rgba(255,255,255,.6);">SUSE, State of hybrid infrastructure, September 2026. Survey of 611 senior IT leaders in the US, UK, France, Germany and Japan.</div>

  <div class="fg logo"><img src="/img/suse-chameleon.png" alt="" /><span>SUSE</span></div>
</div>

<!--
84% of IT leaders admit that at least a quarter of their engineering capacity is consumed by routine platform upkeep: patching, upgrades and compliance evidence. The full survey results are behind the QR code.
-->

---
layout: photo
image: questions-of-trust.jpg
clicks: 3
---

<div style="position:absolute; left:21.5%; top:39%; width:57.5%;">
  <Terminal title="Dockerfile" icon="🐳" :lines="['FROM node:latest']" style="--term-s:2; --term-bp:1.65rem; --term-size:2.45rem; --term-pad:2.3rem 2.4rem 2rem 1.25rem; --term-gap:3.2rem; --term-nw:1ch;" />
</div>

<svg viewBox="0 0 1280 720" style="position:absolute; inset:0; width:100%; height:100%; pointer-events:none;">
  <DrawArrow :on="$clicks >= 1" d="M322 120 C322 250 476 260 476 390" head="M466 373 L476 391 L486 373" />
  <DrawArrow :on="$clicks >= 2" d="M651 120 C651 250 536 260 536 390" head="M526 373 L536 391 L546 373" />
  <DrawArrow :on="$clicks >= 3" d="M982 120 C982 250 676 260 676 390" head="M666 373 L676 391 L686 373" />
</svg>

<span class="chip fx" :class="{ on: $clicks >= 1 }" style="font-size:1.7rem; padding:.55rem 2.1rem; position:absolute; left:25.2%; top:12%; transform:translate(-50%,-50%);">Who built this?</span>
<span class="chip fx" :class="{ on: $clicks >= 2 }" style="font-size:1.7rem; padding:.55rem 2.1rem; position:absolute; left:50.9%; top:12%; transform:translate(-50%,-50%);">What is in there?</span>
<span class="chip fx" :class="{ on: $clicks >= 3 }" style="font-size:1.7rem; padding:.55rem 2.1rem; position:absolute; left:76.7%; top:12%; transform:translate(-50%,-50%);">Which build?</span>

<h1 style="position:absolute; left:0; right:0; top:80%; text-align:center; font-size:4.7rem; margin:0; text-shadow:0 2px 10px rgba(0,0,0,.7);">The Questions of Trust</h1>

<!--
So if we can't just trust our scan and patch cycle, how can we improve? Let's have a look again at the single line of code I wrote.

We need to ask ourselves 3 questions to know whether we can trust this image:
- Who wrote/created the base image?
- What's in there, besides NodeJS?
- You don't even know which version is in there. From pull to pull this can change, depending on whether a new build of the container was released.

These three questions are our ingredients of trust. If we can answer those we know whether we can trust the image, or not.
-->

---
layout: photo
image: anatomy.jpg
clicks: 3
---

<style>
.tag { position:absolute; background:rgba(12,50,44,.78); border-radius:14px; width:31%; height:13.3%; display:grid; place-items:center; font-size:3.1rem; font-weight:700; }
</style>

<div class="tag" style="left:24.5%; top:2.8%; width:51%; font-size:2.65rem;">Anatomy of Trust</div>
<div class="tag fx" :class="{ on: $clicks >= 3 }" style="left:34.5%; top:27.8%;">Attestation</div>
<div class="tag fx" :class="{ on: $clicks >= 1 }" style="left:8.6%; top:66%;">Provenance</div>
<div class="tag fx" :class="{ on: $clicks >= 2 }" style="left:60.4%; top:66%;">Introspection</div>

<!--
In Secure Software Supply Chains these trust questions come down to 3 important concepts to understand.

These are:
- Provenance
- Introspection
- And Attestation

What do these mean? Does anyone know? How do these help you improve on your supply chain security?
-->

---
layout: photo
image: chain-of-custody.jpg
clicks: 1
---

<div style="position:absolute; inset:0; background:rgba(12,50,44,.8);"></div>

<h2 class="ptitle">Chain of Custody</h2>

<div class="fx" :class="{ on: $clicks >= 1 }" style="position:absolute; left:50%; top:30.5%; transform:translate(-50%,-50%); z-index:1; font-size:2.9rem; font-weight:700; display:flex; gap:.8rem; align-items:center;">
  <img src="/img/slsa.png" style="height:3.8rem;" alt="" /> SLSA
</div>

<svg viewBox="0 0 1280 720" style="position:absolute; inset:0; width:100%; height:100%; z-index:1; pointer-events:none;">
  <DrawArrow :on="$clicks >= 1" color="var(--jungle)" :width="4" d="M353 364 C353 310 540 300 588 264" head="M570 267 L588 264 L580 280" />
  <DrawArrow :on="$clicks >= 1" color="var(--jungle)" :width="4" d="M960 364 C960 310 740 300 692 264" head="M710 267 L692 264 L700 280" />
</svg>

<div class="coc" style="left:16%;">
  <div class="h">Provenance</div>
  <div class="accent-green sub">(Origin)</div>
  <div class="pts"><span>→</span> Who built this?<br><span>→</span> Where was it built?<br><span>→</span> Which tools were used?</div>
</div>

<div class="coc" style="left:64%;">
  <div class="h">Attestation</div>
  <div class="accent-green sub">(Proof)</div>
  <div class="pts"><span>→</span> Who signed off?<br><span>→</span> Was it tampered with?</div>
</div>

<style>
.coc { position:absolute; top:53%; z-index:1; }
.coc .h { font-size:3.1rem; font-weight:500; line-height:1.1; }
.coc .sub { font-size:1.55rem; font-weight:700; margin-bottom:.6rem; }
.coc .pts { font-size:1.55rem; line-height:1.74; }
.coc .pts span { color:var(--jungle); margin-right:.9rem; }
</style>

<!--
Let's start off with defining Provenance and Attestation:

Provenance means quite literally "Origin", so in software terms this means in the broadest sense: where was this artifact built.
With Provenance we can get knowledge about the process that was used to get from the source to this artifact. It describes which tools were used to build, and whether the environment was clean at the moment of building. So whether our builds are reproducible. It also tells you who has built this.

So if we have Provenance, we should be able to trust the artifact, correct? Wrong, who says that I'm me and I'm not lying about it? What we need is Attestation.
Attestation means "Proof", i.e. I prove that I'm me, and I certify that I've built this image, and the information in the Provenance report is not tampered with.

Together, Provenance and Attestation make up the SLSA framework, the Supply-chain Levels for Software Artifacts. SLSA levels are a common language to talk about how secure software supply chains and their components are. They are based on industry best practices and look at build, sources and dependencies in software.

What about the Introspection? How does that tie in?
-->

---
layout: photo
image: sbom.jpg
position: 70
---

<h2 class="ptitle">Software Bill of Materials (SBOM)</h2>

<!--
Well Introspection means to be able to look inside. Think of it as adding the label with the ingredients and allergens to your food. You add transparency by listing which versions of which packages are all delivered. This is called a Software Bill of Materials, or SBOM file.

Using the same Attestation key we can also sign this to certify that it is not tampered with.
-->

---
layout: photo
image: trust-baked-in.jpg
---

<h2 style="position:absolute; left:0; right:0; top:10.7%; text-align:center; margin:0; font-size:2.6rem; text-shadow:0 2px 8px rgba(0,0,0,.7);">Trust Baked In</h2>

<div style="position:absolute; left:14%; top:68.7%; text-shadow:0 2px 8px rgba(0,0,0,.8);">
  <div style="font-size:3.1rem; font-weight:500; line-height:1.25;">Provenance<br>&amp; Attestation</div>
  <div class="accent-green" style="font-size:1.6rem; font-weight:700; margin-top:.2rem;">(Process)</div>
</div>
<div style="position:absolute; left:67.7%; top:72.9%; text-shadow:0 2px 8px rgba(0,0,0,.8);">
  <div style="font-size:3.1rem; font-weight:500; line-height:1.25;">SBOM</div>
</div>
<div class="accent-green" style="position:absolute; left:67.7%; top:88.3%; transform:translateY(-50%); font-size:1.6rem; font-weight:700; text-shadow:0 2px 8px rgba(0,0,0,.8);">(Contents)</div>

<!--
Imagine you have a full SLSA Level 3 build of a container, however you have no insight into what is in it. This container can then still be full of vulnerabilities. The factory was clean, but the ingredients were bad.

Vice versa, you can have full insight into the contents with a perfect SBOM file, however the container was on a hacker's laptop and you have no way of telling whether the SBOM file is real or falsified.

Provenance proves the Process was safe. SBOM proves the Contents are known. You need both to trust the software artifact.
-->

---
layout: photo
image: diy.jpg
panel: left
---

# Why DIY is not the Answer

- It’s not just about the Base Container Image
- You need to build the pipeline
- Who will patch your `glibc`?
- You are probably not a Linux maintainer!

---
layout: photo
image: factory.jpg
panel: left
---

# Factory of Trust

- Build and Re-build continuously
- Isolated Builds
- Can you build this?

<!--
The Contrast: "We saw the chaotic kitchen. Now, look at what 'Good' actually looks like. This is a Secure Software Factory."

The Complexity: "To truly solve the 'Mystery Meat' problem, you don't just need a scanner. You need a factory that runs 24/7."

The Requirements: "You need a system that watches upstream code, detects changes, automatically rebuilds the container in a hermetic environment, signs it with a cryptographic key, and generates a 'Nutritional Label' (SBOM)."

The Pivot (the setup for the next slide): "You could build this factory yourself. Companies like Google and Netflix do. But do you want to be in the business of building container factories? Or do you just want the safe containers?"
-->

---
layout: statement
---

<div style="position:absolute; left:14%; right:14%; top:50%; transform:translateY(-50%); text-align:center; font-size:3.65rem; font-weight:500; line-height:1.15;">Run the Kitchen<br><span class="accent-green">Don't</span><br>Manage the Farm.</div>

<!--
"So, we agree. To be safe, you need this sophisticated, hermetic, 24/7 software factory.
But are you in the right place to build it? Wouldn't you rather focus on delivering value to your customers or end-users?

Every minute your smartest engineers spend patching openssl or debugging a build pipeline is a minute they aren't building your product."

So instead of building all of this yourself, you should involve a trusted partner.
-->

---
layout: light
---

# SUSE Base Container Images

<div class="roomy" style="position:absolute; left:5.8%; top:23.8%; width:40%; font-size:1.55rem; --gap:4rem;">

- Secured Base Containers
- Compliance baked in with SLSA Level 3
- Great as a base for anything you develop

</div>
<img src="/img/bci-registry.png" class="shot" style="position:absolute; left:52%; top:21%; width:44%;" alt="SUSE Registry: Trusted Container Images" />

<!--
This is where a company like SUSE can help you. We have ample experience with building a Secure Software Supply Chain, from building and maintaining Linux to securing and observing Cloud Native Clusters.

SUSE has its roots in Open Source and this is also where our heart is. As such, we offer our Base Container Images for free to be used to build your own applications on top of. All of the images in this catalog are free to be redistributed.

Every one comes with full SLSA L3 provenance and attestation and a Software Bill of Materials so you know exactly what's in there.
-->

---
layout: photo
image: michelin.jpg
position: 50
---

<h2 class="ptitle">Don’t Serve a Michelin Burger…</h2>

<div class="tight" style="position:absolute; left:4.7rem; top:10.5rem; font-size:2rem;">

- Your app is now in great shape!

</div>

<!--
We've switched out the base image, our NodeJS application container now comes from a trusted source and has near-0 vulnerabilities. We know who built it and what it contains. Job done, right?
-->

---
layout: photo
image: dumpster-fries.jpg
position: 50
---

<h2 class="ptitle">Don’t Serve a Michelin Burger… With Dumpster Fries</h2>

<div class="tight" style="position:absolute; left:4.7rem; top:10.5rem; font-size:2rem;">

- Your app is now in great shape!
- But you’re Helm installing `redis:latest`

</div>

<!--
Unfortunately, no…. Your application does not live in isolation. That Helm chart you're delivering your application with is also Helm installing redis, a 3rd party database that you don't build or maintain yourself!

So you've now polished that burger, but it comes with mystery fries that don't look too tasty.
-->

---
layout: light
---

# SUSE Application Collection

<div style="position:absolute; left:6.5%; top:24%; width:41%; font-size:1.55rem;">

- Catalog of Open Source Applications
- Full SLSA L3
- SBOM included
- Continuous Vulnerability Scanning

</div>
<img src="/img/appcol-list.png" class="shot" style="position:absolute; left:55.7%; top:3.4%; width:38.9%; height:84%; object-fit:cover; object-position:top;" alt="Application Collection catalog" />
<img src="/img/appcol-detail.png" class="shot" style="position:absolute; left:45.6%; top:15.9%; width:51.7%;" alt="Application Collection: PostgreSQL artifact with SPDX, CycloneDX and SLSA attestations" />

<!--
So, how do we fix the 'Dumpster Fries' problem?
- You could ask your developers to build Postgres from source. (They won't do it.)
- You could ignore the risk. (You shouldn't do it.)

Or you can use the SUSE Application Collection.
We took the top open source applications (Databases, Web Servers, Observability tools) and we put them through our Factory of Trust.

Built on the same secure BCI foundation.

Scanned. Signed. SLSA-verified.

It's the same open source code you love, but with the Chain of Custody you require.

No more mystery meat. No more dumpster dependencies. Just certified ingredients, ready for production.
-->

---
layout: photo
image: gatekeeper.jpg
panel: left
---

# Trust, Beyond the Container

- We now know what trust is, and what you can trust
- How do you secure your platform?
- You need a Gatekeeper

<!--
We have certified ingredients. We have a secure factory. We know what trust is and how we can recognize it.

But Kubernetes doesn't care. By default, Kubernetes will run anything you give it, good or bad. So how do we secure our platform so that no bad code comes in?

We need a Gatekeeper, someone that enforces policies and ensures that no untrusted workloads enter our system.

We need to turn our cluster into a secure platform.
-->

---
layout: light
---

# KubeWarden aka. SUSE Security Admission Controller

<div class="roomy" style="position:absolute; left:5.8%; top:23.2%; width:57.5%; font-size:2rem; --gap:2.9rem;">

- Universal Policy Engine
- Acts as an Admission Controller to allow/deny resources
- CRD Based
- Write a policy in any language that compiles to WASM

</div>
<img src="/img/kubewarden.png" style="position:absolute; left:72.7%; top:22%; width:24%;" alt="Kubewarden logo" />

---
layout: jungle
---

# Blocking non-signed Pods

<div style="position:absolute; left:18.75%; top:14%; width:62.5%;">
  <PolicyWindow />
</div>

---
layout: statement
---

<div style="font-size:5.4rem; line-height:1.1;">Demo time…</div>

---
layout: jungle
logo: false
---

<img src="/img/chef.png" style="position:absolute; left:0; bottom:0; height:81%;" alt="A chef serving a burger" />

<h1 class="ptitle" style="font-weight:500; font-size:2.8rem;">The Only Secret Ingredient… Is Trust</h1>

<div class="checks" style="position:absolute; left:53.6%; top:23%; right:3rem; font-size:2rem;">

- Trust is a Process, not a Scan
- Start with a Secure & Trusted Base
  - SLSA Provenance & Attestation
  - Software Bill of Materials
- Use Trusted Ingredients
- Secure your Platform

</div>

<style>
.checks li { margin-bottom: 2.4rem; padding-left: 2.2rem; }
.checks li::before { content: "✓"; background: none; width: auto; height: auto; top: 0; color: var(--jungle); font-size: 1em; border-radius: 0; }
.checks li ul { margin-top: 0.2rem; font-size: 0.83em; }
.checks li ul li { margin-bottom: 0; line-height: 1.6; }
</style>

---
layout: jungle
---

# What can you do on Monday?

<div class="mrow" style="top:23.4%;">
  <p>Check your container signatures</p>
  <Terminal icon="›_" :lines="['> cosign download signature node:latest']" style="--term-size:1.03rem;" />
</div>
<div class="mrow" style="top:42.8%;">
  <p>Scan your container images</p>
  <Terminal icon="›_" :lines="['> grype node:latest']" style="--term-size:1.03rem;" />
</div>
<div class="mrow" style="top:65.4%;">
  <p style="max-width:27.5rem;">Install KubeWarden in your development cluster</p>
  <Terminal icon="›_" :lines="['> helm repo add kubewarden https://charts.kubewarden.io', '> helm repo update kubewarden', '> helm install --wait -n kubewarden \\', '    --create-namespace admission-controller \\', '    kubewarden/admission-controller']" style="--term-size:0.73rem;" />
</div>

<style>
.mrow { position: absolute; left: 5.8%; right: 13.5%; display: grid; grid-template-columns: 54.1% 45.9%; align-items: start; }
.mrow p { position: relative; margin: 0; padding: 0 0 0 2.9rem; font-size: 2rem; line-height: 1.55; margin-top: -0.2rem; }
.mrow p::before { content: ""; position: absolute; left: 0; top: calc(0.775em - 0.45rem); width: 0.9rem; height: 0.9rem; border-radius: 50%; background: var(--jungle); }
</style>

<!--
Remember that first line of code that made us go down this rabbit hole? "FROM node:latest"? It got you 2000+ vulnerabilities for free.
Well, one line can fix it too. Change that base image to one that comes from a trusted supplier and provides the needed provenance and attestation, and you have secured your first container.

By analyzing the output of these tools on Monday you can make your own decision on which base image you will trust to run in production.
-->

---
layout: jungle
logo: false
bar: short
---

<img src="/img/speaker-closing.png" style="position:absolute; left:0; bottom:0; height:100%;" alt="Jeroen van Erp with a sheep" />

<h1 style="position:absolute; left:50%; top:8%; transform:translate(-50%,-50%); margin:0; font-size:4.05rem; font-weight:500; white-space:nowrap;">Questions?</h1>

<div class="accent-green" style="position:absolute; left:45.8%; top:27%; transform:translateY(-50%); font-size:2.65rem; font-weight:600; white-space:nowrap;">Connect?</div>
<div class="accent-orange" style="position:absolute; left:32.2%; top:41.5%; transform:translateY(-50%); font-size:2.65rem; font-weight:600; white-space:nowrap;">Visit us at Booth 107!</div>

<svg viewBox="0 0 627 353" preserveAspectRatio="none" style="position:absolute; inset:0; width:100%; height:100%; pointer-events:none;" fill="none" stroke-width="2" stroke-linecap="round">
  <g stroke="var(--jungle)"><path d="M384 99 C410 104 428 96 442 86" /><path d="M431 84 L443 85 L440 96" /></g>
</svg>

<img src="/img/qr-linkedin.png" style="position:absolute; left:71.5%; top:2.5%; width:24%;" alt="QR code: connect with Jeroen van Erp on LinkedIn" />
