---
title: Introducing MCP Catalog and Toolkit | Docker
origin_url: https://www.docker.com/blog/introducing-docker-mcp-catalog-and-toolkit/
date: 2025-04-24T15:34:54.002Z
---

# Dockerizing MCP – Bringing Discovery, Simplicity, and Trust to the Ecosystem

!

[Mark Cavage](https://www.docker.com/author/mark-cavage/ "Posts by Mark Cavage")  

  

!

[Tushar Jain](https://www.docker.com/author/tushar-jain/ "Posts by Tushar Jain")  

AI agents are moving fast—from labs to real-world apps. And as they go from generating text to taking real action, the Model Context Protocol (MCP) has emerged as the de facto standard for connecting agents to tools.

MCP is exciting. It’s simple, modular, and built on web-native principles. We believe it has the potential to do for agentic AI interaction what containers did for app deployment – **standardize and simplify** a complex, fragmented landscape.

But, that leaves us at a classic inflection point. MCP Clients and Servers hold enormous potential, but the experience **isn’t production-ready – yet**. Discovery is fragmented, trust is manual, and core capabilities like security and authentication are still patched together with workarounds. 

To move from prototypes to production, a few things need to become non-negotiable. First, developers need a **trusted, centralized hub** to discover tools – no more digging through Discord threads or Twitter replies. And for tool authors, that same hub becomes a **critical distribution channel**: a way to reach new users and ensure compatibility with platforms like Claude, Cursor, OpenAI, and VS Code. Today, that channel simply doesn’t exist. Second, **containerization** should be the default; cloning repos and wrangling dependencies just to get started is unnecessary friction. Third, **credential management** must be seamless and secure – centralized, encrypted, and built to fit modern pipelines. And finally, **security has to be foundational**. Sandbox it. Permission it. Audit it. Trust can’t be an afterthought—it needs to be built in from day one. And it needs to be simple to use: accessible to all developers.

This moment for MCP reminds us a lot of the early days of the cloud and containers – high potential, a few sharp edges, and massive opportunity ahead. These aren’t abstract problems – they’re the same challenges developers face every time a new technology hits its inflection point. We’ve seen it before. And we know how to help. Back in the early days of the cloud, **Docker brought structure** to chaos by making immutability and isolation the standard, building in authentication, and launching Docker Hub as a central discovery layer. It didn’t just **streamline deployment** – it redefined how software gets built, shared, and trusted. Today, Docker serves over **20 million developers** and **powers billions of image** pulls every month. If we bring that same clarity, trust, and scalability to MCP, we unlock a whole new generation of intelligent agents and real-world automation. That’s exactly what we’re doing – with **Docker MCP Catalog and Docker MCP Toolkit**.

And we’re not doing it alone. We’re partnering with leaders like [**Stripe**](https://hub.docker.com/r/mcp/stripe)**,** [**Elastic**](https://hub.docker.com/r/mcp/elasticsearch)**,** [**Heroku**](https://hub.docker.com/r/mcp/heroku)**,** [**Pulumi**](https://hub.docker.com/r/mcp/pulumi)**,** [**Grafana Labs**](https://hub.docker.com/r/mcp/grafana)**,** [**Kong**](https://hub.docker.com/r/mcp/kong) **Inc.,** [**Neo4j**](https://hub.docker.com/r/mcp/neo4j-server)**,** [**New Relic**](http://newrelic.com), [**Continue.dev**](https://www.continue.dev/)**,** and more – each contributing their expertise to help shape a robust, open, and secure MCP ecosystem. This isn’t just another product launch – it’s the foundation of a platform shift. And we’re building it together.

The world we’ve envisioned is one we’re building together with our partners — and it all begins this **May.** Starting then, **the** [**Docker MCP Catalog**](https://hub.docker.com/catalogs/mcp) will serve as the trusted home for discovering MCP tools – seamlessly integrated into Docker Hub. At launch, it will include over **100 verified tools** from leading partners like **Stripe, Elastic, Neo4j, and more**. Each tool will feature publisher verification, versioned releases, and curated collections to help developers find exactly what they need, faster. And just like container images, MCP tools will be distributed via **Docker’s proven pull-based infrastructure** – the same trusted backbone behind billions of downloads every month.

Alongside it, the **Docker MCP Toolkit** brings these tools to life – making them secure, seamless, and instantly usable on your local machine or anywhere Docker runs. With one-click launch from Docker Desktop, you can spin up MCP servers in seconds and connect them to clients like **Docker AI Agent, Claude, Cursor, VS Code, Windsurf,** [**continue.dev**](http://continue.dev/), **and Goose** – no complex setup required. It also includes built-in credentials and OAuth management, integrated with your Docker Hub account, ensuring smooth authentication and making it easy to revoke credentials when necessary. A Gateway MCP Server dynamically exposes enabled tools to compatible clients, while the new docker mcp CLI lets you build, run, and manage them with ease. And with built-in memory, network and disk isolation, every tool runs securely by default-ready for production from day one.

So what does the future look like with Docker MCP Catalog and Toolkit? Picture this: browsing **hundreds of ready-to-run MCP servers** directly on Docker Hub and spinning them up as easily as Redis or Postgres. Instantly connecting them to agents with a few clicks. No more **hardcoded secrets**, no more launching tools with full host access via npx or uvx, and no more **compromising on isolation or security**. Best of all? Run a Docker container, and the MCP tools just work. With familiar commands and tooling, the learning curve is nearly zero—and the possibilities are massive.

Whether you’re building tools, creating agents, or just exploring what’s possible with MCP—we’d love to hear from you. Eager to try the Docker MCP Toolkit and MCP Catalog? Click [here](https://www.docker.com/products/mcp-catalog-and-toolkit/#get_updates) to **join our alert list**. Want a sneak peek? Schedule a session with our DevRel team [here](https://www.docker.com/products/mcp-catalog-and-toolkit/#get_updates). Interested in hosting your own tools on the MCP Catalog? Get in touch with us [**here**](https://www.docker.com/products/mcp-catalog-and-toolkit/#get_updates). Let’s build this **ecosystem** – **together.**

[AI/ML](https://www.docker.com/blog/tag/artificial-intelligence-machine-learning/), [Docker](https://www.docker.com/blog/tag/docker/), [mcp](https://www.docker.com/blog/tag/mcp/)

### Related Posts

#### [Run Gemma 3 with Docker Model Runner: Fully Local GenAI Developer Experience](https://www.docker.com/blog/run-gemma-3-locally-with-docker-model-runner/)

By [Ignasi Lopez Luna](/author/ignasi-lopez-luna/ "Posts by Ignasi Lopez Luna") April 9, 2025

#### [Introducing Docker Model Runner: A Better Way to Build and Run GenAI Models Locally](https://www.docker.com/blog/introducing-docker-model-runner/)

By [Deanna Sparks](/author/deanna-sparks/ "Posts by Deanna Sparks") April 9, 2025

#### [Docker Desktop 4.40: Model Runner to run LLMs locally, more powerful Docker AI Agent, and expanded AI Tools Catalog](https://www.docker.com/blog/docker-desktop-4-40/)

By [Yiwen Xu](/author/yiwen-xu/ "Posts by Yiwen Xu") April 1, 2025

#### Posted

Apr 22, 2025

*   [.cls-1{fill:#1d63ed;}](https://twitter.com/intent/tweet?url=https%3A%2F%2Fwww.docker.com%2Fblog%2Fintroducing-docker-mcp-catalog-and-toolkit%2F)
*   [.cls-1{fill:#1d63ed;}](https://www.linkedin.com/sharing/share-offsite/?url=https%3A%2F%2Fwww.docker.com%2Fblog%2Fintroducing-docker-mcp-catalog-and-toolkit%2F)
*   [.cls-1{fill:#1d63ed;}](https://www.facebook.com/sharer/sharer.php?u=https%3A%2F%2Fwww.docker.com%2Fblog%2Fintroducing-docker-mcp-catalog-and-toolkit%2F)

#### Post Tags

[AI/ML](https://www.docker.com/blog/tag/artificial-intelligence-machine-learning/)[Docker](https://www.docker.com/blog/tag/docker/)[mcp](https://www.docker.com/blog/tag/mcp/)

#### Categories

*   [Community](https://www.docker.com/blog/category/community-content/)
*   [Company](https://www.docker.com/blog/category/company/)
*   [Engineering](https://www.docker.com/blog/category/engineering/)
*   [Products](https://www.docker.com/blog/category/products/)

.cls-1 { fill: #1d63ed; stroke-width: 0px; }

*   [Products](#)
    *   [Docker Desktop](/products/docker-desktop/)
    *   [Docker Hub](/products/docker-hub/)
    *   [Docker Scout](https://www.docker.com/products/docker-scout/)
    *   [Docker Build Cloud](/products/build-cloud/)
    *   [Features](#)
    *   [Command Line Interface](/products/cli/)
    *   [IDE Extensions](/products/ide/)
    *   [Container Runtime](/products/container-runtime/)
    *   [Docker Extensions](https://www.docker.com/products/extensions/)
    *   [Trusted Open Source Content](/products/trusted-content/open-source/)
    *   [Secure Software Supply Chain](https://www.docker.com/solutions/security/)
*   [Developers](#)
    *   [Documentation](https://docs.docker.com/)
    *   [Getting Started](/get-started/)
    *   [Trainings](/resources/trainings/)
    *   [Extensions SDK](/developers/sdk/)
    *   [Community](/community/)
    *   [Open Source](/community/open-source/)
    *   [Preview Program](/community/get-involved/developer-preview/)
*   [Pricing](/pricing/)
    *   [Personal](/products/personal/)
    *   [Pro](/products/pro/)
    *   [Team](/products/team/)
    *   [Business](/products/business/)
    *   [Pricing FAQ](/pricing/faq/)
    *   [Contact Sales](/pricing/contact-sales/)
*   [Support](https://www.docker.com/support/)
    *   [Docker System Status](http://dockerstatus.com/)
*   [Blog](/blog/)
    *   [Newsletter](/newsletter-subscription/)
*   [Company](/company/)
    *   [About Us](/company/)
    *   [What is a Container](/resources/what-container/)
    *   [Why Docker](/why-docker/)
    *   [Trust](/trust/)
    *   [Customer Success](/customer-success/)
    *   [Partners](/partners/)
    *   [Events](/events/)
    *   [Newsroom](/company/newsroom/)
    *   [Swag Store](https://stores.kotisdesign.com/docker)
    *   [Brand Guidelines](/company/newsroom/media-resources/)
    *   [Trademark Guidelines](/legal/trademark-guidelines/)
    *   [Careers](/careers/)
    *   [Contact Us](/company/contact/)
    *   [Languages](#)
        *   [English](https://www.docker.com/)
        *   [日本語](https://www.docker.com/ja-jp/)

*   [![twitter logo](/app/themes/Ponyo-divi-child/dist/images/social/docker-social-twitter.svg)](http://twitter.com/docker)
*   [![linkedin logo](/app/themes/Ponyo-divi-child/dist/images/social/docker-social-linkedin.svg)](https://www.linkedin.com/company/docker)
*   [![instagram logo](/app/themes/Ponyo-divi-child/dist/images/social/docker-social-instagram.svg)](https://www.instagram.com/dockerinc/)
*   [![youtube logo](/app/themes/Ponyo-divi-child/dist/images/social/docker-social-youtube.svg)](http://www.youtube.com/user/dockerrun)
*   [![facebook logo](/app/themes/Ponyo-divi-child/dist/images/social/docker-social-facebook.svg)](https://www.facebook.com/docker.run)
*   [![rss feed logo](/app/themes/Ponyo-divi-child/dist/images/social/docker-social-blog.svg)](/blog/feed)

© 2025 Docker Inc. All rights reserved|[Terms of Service](/legal/docker-terms-service)|[Privacy](/legal/privacy)|[Legal](/legal)

Cookie Settings

#footer { background-color: #F9F9FA !important; } .footer-color-white #footer { background-color: #fff !important; } .is-small-text{font-size:.875em}.is-regular-text{font-size:1em}.is-large-text{font-size:2.25em}.is-larger-text{font-size:3em}.has-drop-cap:not(:focus):first-letter{float:left;font-size:8.4em;font-style:normal;font-weight:100;line-height:.68;margin:.05em .1em 0 0;text-transform:uppercase}body.rtl .has-drop-cap:not(:focus):first-letter{float:none;margin-left:.1em}p.has-drop-cap.has-background{overflow:hidden}:root :where(p.has-background){padding:1.25em 2.375em}:where(p.has-text-color:not(.has-link-color)) a{color:inherit}p.has-text-align-left\[style\*="writing-mode:vertical-lr"\],p.has-text-align-right\[style\*="writing-mode:vertical-rl"\]{rotate:180deg} ( function() { var skipLinkTarget = document.querySelector( 'main' ), sibling, skipLinkTargetID, skipLink; // Early exit if a skip-link target can't be located. if ( ! skipLinkTarget ) { return; } /\* \* Get the site wrapper. \* The skip-link will be injected in the beginning of it. \*/ sibling = document.querySelector( '.wp-site-blocks' ); // Early exit if the root element was not found. if ( ! sibling ) { return; } // Get the skip-link target's ID, and generate one if it doesn't exist. skipLinkTargetID = skipLinkTarget.id; if ( ! skipLinkTargetID ) { skipLinkTargetID = 'wp--skip-link--target'; skipLinkTarget.id = skipLinkTargetID; } // Create the skip link. skipLink = document.createElement( 'a' ); skipLink.classList.add( 'skip-link', 'screen-reader-text' ); skipLink.href = '#' + skipLinkTargetID; skipLink.innerHTML = 'Skip to content'; // Inject the skip link. sibling.parentElement.insertBefore( skipLink, sibling ); }() ); var pp\_ajax\_form = {"ajaxurl":"https:\\/\\/www.docker.com\\/wp\\/wp-admin\\/admin-ajax.php","confirm\_delete":"Are you sure?","deleting\_text":"Deleting...","deleting\_error":"An error occurred. Please try again.","nonce":"d373248124","disable\_ajax\_form":"false","is\_checkout":"0","is\_checkout\_tax\_enabled":"0","is\_checkout\_autoscroll\_enabled":"true"}; jQuery(function(jQuery){jQuery.datepicker.setDefaults({"closeText":"Close","currentText":"Today","monthNames":\["January","February","March","April","May","June","July","August","September","October","November","December"\],"monthNamesShort":\["Jan","Feb","Mar","Apr","May","Jun","Jul","Aug","Sep","Oct","Nov","Dec"\],"nextText":"Next","prevText":"Previous","dayNames":\["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"\],"dayNamesShort":\["Sun","Mon","Tue","Wed","Thu","Fri","Sat"\],"dayNamesMin":\["S","M","T","W","T","F","S"\],"dateFormat":"MM d, yy","firstDay":1,"isRTL":false});}); var DIVI = {"item\_count":"%d Item","items\_count":"%d Items"}; var et\_builder\_utils\_params = {"condition":{"diviTheme":true,"extraTheme":false},"scrollLocations":\["app","top"\],"builderScrollLocations":{"desktop":"app","tablet":"app","phone":"app"},"onloadScrollLocation":"app","builderType":"fe"}; var et\_frontend\_scripts = {"builderCssContainerPrefix":"#et-boc","builderCssLayoutPrefix":"#et-boc .et-l"}; var et\_pb\_custom = {"ajaxurl":"https:\\/\\/www.docker.com\\/wp\\/wp-admin\\/admin-ajax.php","images\_uri":"https:\\/\\/www.docker.com\\/app\\/themes\\/Divi\\/images","builder\_images\_uri":"https:\\/\\/www.docker.com\\/app\\/themes\\/Divi\\/includes\\/builder\\/images","et\_frontend\_nonce":"d6865c4773","subscription\_failed":"Please, check the fields below to make sure you entered the correct information.","et\_ab\_log\_nonce":"29d9895a5a","fill\_message":"Please, fill in the following fields:","contact\_error\_message":"Please, fix the following errors:","invalid":"Invalid email","captcha":"Captcha","prev":"Prev","previous":"Previous","next":"Next","wrong\_captcha":"You entered the wrong number in captcha.","wrong\_checkbox":"Checkbox","ignore\_waypoints":"no","is\_divi\_theme\_used":"1","widget\_search\_selector":".widget\_search","ab\_tests":\[\],"is\_ab\_testing\_active":"","page\_id":"69742","unique\_test\_id":"","ab\_bounce\_rate":"5","is\_cache\_plugin\_active":"yes","is\_shortcode\_tracking":"","tinymce\_uri":"https:\\/\\/www.docker.com\\/app\\/themes\\/Divi\\/includes\\/builder\\/frontend-builder\\/assets\\/vendors","accent\_color":"#7EBEC5","waypoints\_options":\[\]}; var et\_pb\_box\_shadow\_elements = \[\]; var gforms\_recaptcha\_recaptcha\_strings = {"site\_key":"6LcGUeoqAAAAAEQQG63wGXQsUtH\_R84IlGi\_2WPS","ajaxurl":"https:\\/\\/www.docker.com\\/wp\\/wp-admin\\/admin-ajax.php","nonce":"30e43bbaa1"}; (function($){grecaptcha.ready(function(){$('.grecaptcha-badge').css('visibility','hidden');});})(jQuery); .et\_pb\_row\_0\_tb\_body,body #page-container .et-db #et-boc .et-l .et\_pb\_row\_0\_tb\_body.et\_pb\_row,body.et\_pb\_pagebuilder\_layout.single #page-container #et-boc .et-l .et\_pb\_row\_0\_tb\_body.et\_pb\_row,body.et\_pb\_pagebuilder\_layout.single.et\_full\_width\_page #page-container #et-boc .et-l .et\_pb\_row\_0\_tb\_body.et\_pb\_row{width:88%;max-width:1140px}.et\_pb\_text\_0\_tb\_body,.et\_pb\_text\_0\_tb\_body h5,.et\_pb\_text\_1\_tb\_body h5,.et\_pb\_text\_2\_tb\_body h5{font-size:20px}.et\_pb\_text\_0\_tb\_body.et\_pb\_text ul li,.et\_pb\_text\_0\_tb\_body.et\_pb\_text ol li>ul li,.et\_pb\_text\_1\_tb\_body.et\_pb\_text ul li,.et\_pb\_text\_1\_tb\_body.et\_pb\_text ol li>ul li,.et\_pb\_text\_2\_tb\_body.et\_pb\_text ul li,.et\_pb\_text\_2\_tb\_body.et\_pb\_text ol li>ul li{color:#697a89!important}.et\_pb\_text\_0\_tb\_body ul li,.et\_pb\_text\_1\_tb\_body ul li,.et\_pb\_text\_2\_tb\_body ul li{line-height:1.5em;line-height:1.5em}.et\_pb\_text\_0\_tb\_body h2,.et\_pb\_text\_1\_tb\_body h2,.et\_pb\_text\_2\_tb\_body h2{font-size:44px}.et\_pb\_text\_0\_tb\_body h3,.et\_pb\_text\_1\_tb\_body h3{font-size:28px;color:#0b214a!important}.et\_pb\_text\_0\_tb\_body h4,.et\_pb\_text\_1\_tb\_body h4,.et\_pb\_text\_2\_tb\_body h4{font-size:24px}.et\_pb\_text\_0\_tb\_body h6,.et\_pb\_text\_1\_tb\_body h6,.et\_pb\_text\_2\_tb\_body h6{font-size:18px}.et\_pb\_post\_content\_0\_tb\_body,.et\_pb\_text\_1\_tb\_body,.et\_pb\_text\_2\_tb\_body{font-size:17px}.et\_pb\_post\_content\_0\_tb\_body h3{font-weight:700}.et\_pb\_text\_2\_tb\_body h3{font-size:32px;color:gcid-897cbb42-a3a1-405b-a20d-0dc643039d74!important}.et\_pb\_sidebar\_0\_tb\_body{width:100%}.et\_pb\_sidebar\_0\_tb\_body.et\_pb\_widget\_area{display:flex;flex-direction:column}.et\_pb\_section\_0\_tb\_footer.et\_pb\_section{padding-top:0px}.et\_pb\_row\_2\_tb\_footer{border-bottom-width:1px;border-bottom-color:#efeff2}.et\_pb\_row\_2\_tb\_footer.et\_pb\_row{padding-top:0px!important;padding-top:0px}.et\_pb\_row\_2\_tb\_footer,body #page-container .et-db #et-boc .et-l .et\_pb\_row\_2\_tb\_footer.et\_pb\_row,body.et\_pb\_pagebuilder\_layout.single #page-container #et-boc .et-l .et\_pb\_row\_2\_tb\_footer.et\_pb\_row,body.et\_pb\_pagebuilder\_layout.single.et\_full\_width\_page #page-container #et-boc .et-l .et\_pb\_row\_2\_tb\_footer.et\_pb\_row,.et\_pb\_row\_3\_tb\_footer,body #page-container .et-db #et-boc .et-l .et\_pb\_row\_3\_tb\_footer.et\_pb\_row,body.et\_pb\_pagebuilder\_layout.single #page-container #et-boc .et-l .et\_pb\_row\_3\_tb\_footer.et\_pb\_row,body.et\_pb\_pagebuilder\_layout.single.et\_full\_width\_page #page-container #et-boc .et-l .et\_pb\_row\_3\_tb\_footer.et\_pb\_row{width:88%;max-width:1280px}.et\_pb\_code\_1\_tb\_footer{background-size:110px;background-position:2% 0px}.et\_pb\_row\_3\_tb\_footer.et\_pb\_row{padding-top:0px!important;padding-bottom:0px!important;padding-top:0px;padding-bottom:0px}.et\_pb\_text\_0\_tb\_footer,.et\_pb\_text\_0\_tb\_footer h5{font-size:20px}.et\_pb\_text\_0\_tb\_footer.et\_pb\_text ul li,.et\_pb\_text\_0\_tb\_footer.et\_pb\_text ol li>ul li{color:#697a89!important}.et\_pb\_text\_0\_tb\_footer ul li{line-height:1.5em;line-height:1.5em}.et\_pb\_text\_0\_tb\_footer h2{font-size:44px}.et\_pb\_text\_0\_tb\_footer h3{font-size:28px;color:#0b214a!important}.et\_pb\_text\_0\_tb\_footer h4{font-size:24px}.et\_pb\_text\_0\_tb\_footer h6{font-size:18px}.et\_pb\_code\_3\_tb\_footer{text-align:right}.et\_pb\_column\_2\_tb\_footer{padding-top:20px}@media only screen and (max-width:980px){.et\_pb\_row\_2\_tb\_footer{border-bottom-width:1px;border-bottom-color:#efeff2}.et\_pb\_row\_3\_tb\_footer{flex-direction:column-reverse;display:flex}.et\_pb\_column\_1\_tb\_footer{padding-bottom:42px}}@media only screen and (max-width:767px){.et\_pb\_row\_2\_tb\_footer{border-bottom-width:1px;border-bottom-color:#efeff2}.et\_pb\_row\_3\_tb\_footer{flex-direction:column-reverse;display:flex}.et\_pb\_column\_1\_tb\_footer{padding-bottom:42px}} window.NREUM||(NREUM={});NREUM.info={"beacon":"bam.nr-data.net","licenseKey":"NRJS-27f33ade91093c8b2a2","applicationID":"1254123379","transactionName":"NgYEbEpYW0ZQB0MPCw9MJ1tMUFpbHgJFCQoVBghcFVtaUUhJQwMJEQ8HTF0=","queueTime":0,"applicationTime":1598,"atts":"GkEHGgJCSEg=","errorBeacon":"bam.nr-data.net","agent":""}