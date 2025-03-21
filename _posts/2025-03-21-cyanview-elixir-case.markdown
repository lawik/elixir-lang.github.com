---
layout: post
title: "Remote: growing from zero to unicorn with Elixir"
authors:
- Hugo Baraúna
category: Elixir in Production
excerpt: A case study of how Elixir is being used at Remote.
logo: /images/cases/logos/remote.png
tags: growth team web
---

*Welcome to our series of [case studies about companies using Elixir in production](/cases.html).*

<!-----

You have some errors, warnings, or alerts. If you are using reckless mode, turn it off to see useful information and inline alerts.
* ERRORs: 0
* WARNINGs: 0
* ALERTS: 4

Conversion time: 1.749 seconds.


Using this Markdown file:

1. Paste this output into your source file.
2. See the notes and action items below regarding this conversion run.
3. Check the rendered output (headings, lists, code blocks, tables) for proper
   formatting and use a linkchecker before you publish this page.

Conversion notes:

* Docs to Markdown version 1.0β44
* Fri Mar 21 2025 07:41:24 GMT-0700 (PDT)
* Source doc: CyanView - Case Study
* This document has images: check for >>>>>  gd2md-html alert:  inline image link in generated source and store images to your server. NOTE: Images in exported zip file from Google Docs may not appear in  the same order as they do in your doc. Please check the images!

----->


<p style="color: red; font-weight: bold">>>>>>  gd2md-html alert:  ERRORs: 0; WARNINGs: 0; ALERTS: 4.</p>
<ul style="color: red; font-weight: bold"><li>See top comment block for details on ERRORs and WARNINGs. <li>In the converted Markdown or HTML, search for inline alerts that start with >>>>>  gd2md-html alert:  for specific instances that need correction.</ul>

<p style="color: red; font-weight: bold">Links to alert messages:</p><a href="#gdcalert1">alert1</a>
<a href="#gdcalert2">alert2</a>
<a href="#gdcalert3">alert3</a>
<a href="#gdcalert4">alert4</a>

<p style="color: red; font-weight: bold">>>>>> PLEASE check and correct alert issues and delete this message and the inline alerts.<hr></p>



# Coordinating Super Bowl's visual fidelity with Cyanview and Elixir

How do you coordinate visual fidelity across two hundred cameras for a live event like the Super Bowl?

The answer is: by using the craft of camera shading, which involves adjusting each camera to ensure they match up in color, exposure and various other visual aspects. The goal is to turn the live event broadcast into a cohesive and consistent experience. For every angle used, you want the same green grass and the same skin tones. Everything needs to be very closely tuned across a diverse set of products and brands. From large broadcast cameras, drone cameras, and PTZ cameras to gimbal-held mirrorless cameras and more. This is what Cyanview does. Cyanview is a small Belgian company that sells products for the live video broadcast industry, and its primary focus is shading.

Broadcast is a business where you only get one chance to prove that your tool is up to the task. Reliability is king. There can be no hard failures.

A small team of three built a product so powerful and effective that it spread across the industry purely on the strength of its functionality. Without any marketing, it earned a reputation among seasoned professionals and became a staple at the world’s top live events. Cyanview's Remote Control Panel (RCP) is now used by specialist video operators on the Olympics, Super Bowl, NFL, NBA, ESPN, Amazon and many more. Even most fashion shows in Paris use Cyanview’s devices.

These devices put Elixir right in the critical path for serious broadcast operations. By choosing Elixir, Cyanview gained best-in-class networking features, state-of-the-art resilience and an ecosystem that allowed fast iteration on product features.



<p id="gdcalert1" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image1.jpg). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert2">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image1.jpg "image_tooltip")



## Why Elixir?

The founding team of Cyanview primarily had experience with embedded development, and the devices they produce involve a lot of low-level C code and plenty of FPGA. This is due to the low-level details of color science and the really tight timing requirements.

If you’ve ever worked with camera software, you know it can be a mixed bag. Even after going fully digital, much of it remained tied to analog systems or relied on proprietary connectivity solutions. Cyanview has been targeting IP (as in Internet Protocol) from early on. This means Cyanview's software can operate on commodity networks that work in well-known and well-understood ways. This has aligned well with an increase in remote production, partially due to the pandemic, where production crews operate from a central location with minimal crew on location. Custom radio frequency or serial wire protocols have a hard time scaling to cross-continent distances.

This also paved the way for Elixir, as the Erlang VM was designed to communicate and coordinate millions of devices, reliably, over the network.

Elixir was brought in by the developer Ghislain, who needed to build integrations with cameras and interact with the other bits of required video gear, with many different protocols over the network. The language comes with a lot of practical features for encoding and decoding binary data down to the individual bits. Elixir gave them a strong foundation and the tools to iterate fast.

Ghislain has been building the core intellectual property of Cyanview ever since. While the physical device naturally has to be solid, reliable, and of high quality, a lot of the secret sauce ultimately lies in the massive number of integrations and huge amounts of reverse engineering. Thus, the product is able to work with as many professional camera systems and related equipment as possible. It is designed to be compatible with everything and anything a customer is using. Plus, it offers an API to ensure smooth integration with other devices.

David Bourgeois, the founder of Cyanview, told us a story how these technical decisions alongside Elixir helped them tackle real-world challenges:

"During the Olympics in China, a studio in Beijing relied on a large number of Panasonic PTZ cameras. Most of their team, however, was based in Paris and needed to control the cameras remotely to run various shows throughout the day. The problem? Panasonic’s camera protocols were never designed for internet use — they require precise timing and multiple messages for every adjustment. With network latency, that leads to timeouts, disconnects, and system failures... So they ended up placing our devices next to the cameras in Beijing and controlled them over IP from Paris — just as designed."



<p id="gdcalert2" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image2.jpg). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert3">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image2.jpg "image_tooltip")


The devices in a given location communicate and coordinate on the network over a custom MQTT protocol. Over a hundred cameras without issue on a single Remote Control Panel (RCP), implemented on top of Elixir's network stack.


## Technical composition

The system as a whole consists of RCP devices running a Yocto Linux system, with most of the logic built in Elixir and C. While Python is still used for scripting and tooling, its role has gradually diminished. The setup also includes multiple microcontrollers and the on-camera device, all communicating over MQTT. Additionally, cloud relays facilitate connectivity, while dashboards and controller UIs provide oversight and control. The two critical devices are the RCP offering control on the production end and the RIO handling low-latency manipulation of the camera. Both run Elixir.

The configuration UI is currently built in Elm, but - depending on priorities - it might be converted to [Phoenix LiveView](https://phoenixframework.org/) over time to reduce the number of languages in use. The controller web UI is already in LiveView, and it is performing quite well on a very low-spec embedded Linux machine.

The cloud part of the system is very limited today, which is unusual in a world of SaaS. There are cloud relays for distributing and sharing camera control as well as forwarding network ports between locations and some related features, also built in Elixir, but cloud is not at the core of the business. The devices running Elixir on location form a cluster over IP using a custom MQTT-based protocol suited to the task and are talking to hundreds of cameras and other video devices.

It goes without saying that integration with so much proprietary equipment comes with challenges. Some integrations are more reliable than others. Some devices are more common, and their quirks are well-known through hard-won experience. A few even have good documentation that you can reference while others offer mystery and constant surprises. In this context, David emphasizes the importance of Elixir's mechanisms for recovering from failures:

"If one camera connection has a blip, a buggy protocol or the physical connection to a device breaks it is incredibly important that everything else keeps working. And this is where Elixir’s supervision trees provide a critical advantage."


## Growth & team composition

The team has grown over the 9 years that the company has been operating, but it did so at a slow and steady pace. On average, the company has added just one person per year. With nine employees at the time of writing, Cyanview supports some of the biggest broadcast events in the world. .

There are two Elixir developers on board: Daniil who is focusing on revising some of the UI as well as charting a course into more cloud functionality, and Ghislain, who works on cameras and integration. Both LiveView and Elm are used to power device UIs and dashboards.

What’s interesting is that, overall, the other embedded developers say that they don't know much about Elixir and they don't use it in their day-to-day work. Nonetheless, they are very comfortable implementing protocols and encodings in Elixir. The main reason they haven’t fully learned the language is simply time — they have plenty of other work to focus on, and deep Elixir expertise hasn’t been necessary. After all, there’s much more to their work beyond Elixir: designing PCBs, selecting electronic components, reverse engineering protocols, interfacing with displays, implementing FPGAs, managing production tests, real productions and releasing firmware updates.


## Innovation and customer focus

<p id="gdcalert3" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image3.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert4">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image3.png "image_tooltip")
 

Whether it’s providing onboard cameras in 40+ cars during the 24 hours of Le Mans, covering Ninja Warrior, the Australian Open, and the US Open, operating a studio in the Louvre, being installed in NFL pylons, or connecting over 200 cameras simultaneously the – product speaks for itself. Cyanview built a device for a world that runs on top of IP, using Elixir, a language with networking and protocols deep in its bones. This choice enabled them to do both: implement support for all the equipment and provide features no one else had.

By shifting from conventional local-area radio frequency, serial connections, and inflexible proprietary protocols to IP networking, Cyanview’s devices redefined how camera systems operate. Their feature set is unheard of in the industry: Unlimited multicam. Tally lights. Pan & Tilt control. Integration with color correctors. World-spanning remote production.

The ease and safety of shipping new functionality have allowed the company to support new features very quickly. One example is the increasing use of mirrorless cameras on gimbals to capture crowd shots. Cyanview were able to prototype gimbal control, test it with a customer and validate that it worked in a very short amount of time. This quick prototyping and validation of features is made possible by a flexible architecture that ensures that critical fundamentals don't break.

Camera companies that don't produce broadcast shading remotes, such as Canon or RED, recommend Cyanview to their customers. Rather than competing with most broadcast hardware companies, Cyanview considers itself a partner. The power of a small team, a quality product and powerful tools can be surprising. Rather than focusing on marketing, Cyanview works very closely with its customers by supporting the success of their events and providing in-depth customer service.


## Looking back and forward

When asked if he would choose Elixir again, David responded:

"Yes. We've seen what the Erlang VM can do, and it has been very well-suited to our needs. You don't appreciate all the things Elixir offers out of the box until you have to try to implement them yourself. It was not pure luck that we picked it, but we were still lucky. Elixir turned out to bring a lot that we did not know would be valuable to us. And we see those parts clearly now."

Cyanview hopes to grow the team more, but plans to do so responsibly over time. Currently there is a lot more to do than the small team can manage.

Development is highly active, with complementary products already in place alongside the main RCP device, and the future holds even more in that regard. Cloud offerings are on the horizon, along with exciting hardware projects that build on the lessons learned so far. As these developments unfold, we’ll see Elixir play an increasingly critical role in some of the world’s largest live broadcasts.



<p id="gdcalert4" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image4.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert5">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image4.png "image_tooltip")



## In summary

A high-quality product delivering the right innovation at the right time in an industry that's been underserved in terms of good integration. Elixir provided serious leverage for developing a lot of integrations with high confidence and consistent reliability. In an era where productivity and lean, efficient teams are everything, Cyanview is a prime example of how Elixir empowers small teams to achieve an outsized impact.




REMOTE FOR REFERENCE
---


Remote is the everywhere employment platform enabling companies to find, hire, manage, and pay people anywhere across the world.

Founded in 2019, they reached unicorn status in just over two years and have continued their rapid growth trajectory since.

Since day zero, Elixir has been their primary technology. Currently, their engineering organization as a whole consists of nearly 300 individuals.

This case study focuses on their experience using Elixir in a high-growth environment.

![Remote website screenshot](/images/cases/bg/remote.png)

## Why Elixir?

Marcelo Lebre, co-founder and president of Remote, had worked with many languages and frameworks throughout his career, often encountering the same trade-off: easy-to-code versus easy-to-scale.

In 2015, while searching for alternatives, he discovered Elixir. Intrigued, Marcelo decided to give it a try and immediately saw its potential. At the time, Elixir was still in its early days, but he noticed how fast the community was growing, with support for packages and frameworks starting to show up aggressively.

In December 2018, when Marcelo and his co-founder decided to start the company, they had to make a decision about the technology that would support their vision. Marcelo wanted to prioritize building a great product quickly without worrying about scalability issues from the start. He found Elixir to be the perfect match:

> I wanted to focus on building a great product fast and not really worry about its scalability. Elixir was the perfect match—reliable performance, easy-to-read syntax, strong community, and a learning curve that made it accessible to new hires.
>
> \- 	Marcelo Lebre, Co-founder and President

The biggest trade-off Marcelo identified was the smaller pool of Elixir developers compared to languages like Ruby or Python. However, he quickly realized that the quality of candidates more than made up for it:

> The signal-to-noise ratio in the quality of Elixir candidates was much higher, which made the trade-off worthwhile.
>
> \- 	Marcelo Lebre, Co-founder and President

## Growing with a monolithic architecture

Remote operates primarily with a monolith, with Elixir in the backend and React in the front-end.

The monolith enabled speed and simplicity, allowing the team to iterate quickly and focus on building features. However, as the company grew, they needed to invest in tools and practices to manage almost 180 engineers working in the same codebase.

One practice was boundary enforcement. They used the [Boundary library](https://github.com/sasa1977/boundary) to maintain strict boundaries between modules and domains inside the codebase.

Another key investment was optimizing their compilation time in the CI pipeline. Since their project has around 15,000 files, compiling it in every build would take too long. So, they implemented incremental builds in their CI pipeline, recompiling only the files affected by changes instead of the entire codebase.

> I feel confident making significant changes in the codebase. The combination of using a functional language and our robust test suite allows us to keep moving forward without too much worry.
>
> \- André Albuquerque, Staff Engineer

Additionally, as their codebase grew, the Elixir language continued to evolve, introducing better abstractions for developers working with large codebases. For example, with the release of Elixir v1.11, the [introduction of config/runtime.exs](/blog/2020/10/06/elixir-v1-11-0-released/) provided the Remote team with a better foundation for managing configuration. This enabled them to move many configurations from compile-time to runtime, significantly reducing unnecessary recompilations caused by configuration updates.

## Infra-structure and operations

One might expect Remote’s infrastructure to be highly complex, given their global scale and the size of their engineering team. Surprisingly, their setup remains relatively simple, reflecting a thoughtful balance between scalability and operational efficiency.

Remote runs on AWS, using EKS (Elastic Kubernetes Service). The main backend (the monolith) operates in only five pods, each with 10 GB of memory. They use [Distributed Erlang](https://www.erlang.org/doc/system/distributed.html) to connect the nodes in their cluster, enabling seamless communication between processes running on different pods.

For job processing, they rely on [Oban](https://github.com/oban-bg/oban), which runs alongside the monolith in the same pods.

Remote also offers a public API for partners. While this API server runs separately from the monolith, it is the same application, configured to start a different part of its supervision tree. The separation was deliberate, as the team anticipated different load patterns for the API and wanted the flexibility to scale it independently.

The database setup includes a primary PostgreSQL instance on AWS RDS, complemented by a read-replica for enhanced performance and scalability. Additionally, a separate Aurora PostgreSQL instance is dedicated to storing Oban jobs. Over time, the team has leveraged tools like PG Analyze to optimize performance, addressing bottlenecks such as long queries and missing indexes.

This streamlined setup has proven resilient, even during unexpected spikes in workload. The team shared an episode where a worker’s job count unexpectedly grew by two orders of magnitude. Remarkably, the system handled the increase seamlessly, continuing to run as usual without requiring any design changes or manual intervention.

> We once noticed two weeks later that a worker’s load had skyrocketed. But the scheduler worked fine, and everything kept running smoothly. That was fun.
>
> \- Alex Naser, Staff Engineer

## Team organization and responsibilities

Around 90% of their backend team works in the monolith, while the rest work in a few satellite services, also written in Elixir.

Within the monolith, teams are organized around domains such as onboarding, payroll, and billing. Each team owns one or multiple domains.

To streamline accountability in a huge monolith architecture, Remote invested heavily in team assignment mechanisms.

They implemented a tagging system that assigns ownership down to the function level. This means any trace—whether sent to tools like Sentry or Datadog—carries a tag identifying the responsible team. This tagging also extends to endpoints, allowing teams to monitor their areas effectively and even set up dashboards for alerts, such as query times specific to their domain.

The tagging system also simplifies CI workflows. When a test breaks, it’s automatically linked to the responsible team based on the Git commit. This ensures fast issue identification and resolution, removing the need for manual triaging.

## Hiring and training

Remote’s hiring approach prioritizes senior engineers, regardless of their experience with Elixir.

During the hiring process, all candidates are required to complete a coding exercise in Elixir. For those unfamiliar with the language, a tailored version of the exercise is provided, designed to introduce them to Elixir while reflecting the challenges they would face if hired.

Once hired, new engineers are assigned an engineering buddy to guide them through the onboarding process.

For hires without prior Elixir experience, Remote developed an internal Elixir training camp, a curated collection of best practices, tutorials, and other resources to introduce new hires to the language and ecosystem. This training typically spans two to four weeks.

After completing the training, engineers are assigned their first tasks—carefully selected tickets designed to build confidence and familiarity with the codebase.

## Summing up

Remote’s journey highlights how thoughtful technology, infrastructure, and team organization decisions can support rapid growth.

By leveraging Elixir’s strengths, they built a monolithic architecture that balanced simplicity with scalability. This approach allowed their engineers to iterate quickly in the early stages while effectively managing the complexities of a growing codebase.

Investments in tools like the Boundary library and incremental builds ensured their monolith remained efficient and maintainable even as the team and codebase scaled dramatically.

Remote's relatively simple infrastructure demonstrates that scaling doesn't always require complexity. Their ability to easily handle unexpected workload spikes reflects the robustness of their architecture and operational practices.

Finally, their focus on team accountability and streamlined onboarding allowed them to maintain high productivity while integrating engineers from diverse technical backgrounds, regardless of their prior experience with Elixir.
