1. The Metadata Model (The <prolog>)

Every topic in your knowledgebase should include standard metadata. This ensures you can later filter content (e.g., generating a beginner vs. advanced course). DITA uses the <prolog> element for this.

Standard Topic Prolog:
<prolog>
    <author type="creator">Your Name</author>
    <critdates>
        <created date="2026-02-23"/>
        <revised modified="2026-03-10"/>
    </critdates>
    <metadata>
        <!-- Defines who this is for -->
        <audience type="user" job="translating" experiencelevel="intermediate"/>
        <!-- Useful for grouping/searching in your KB -->
        <category>Terminology Extraction</category>
        <category>NLP</category>
    </metadata>
</prolog>
2. The Information Types (Topic Models)

Based on your example, your content falls naturally into DITA's core topic types. By separating what it is (Concept) from how to do it (Task), your knowledgebase becomes highly reusable.
A. Concept Topics (The "Conceptual Work")

Used for theory, explanations, and background information.

    Rule for your model: Every Concept must include an <example> element and reference citations.

    Examples from your course: What is a Term?, Defining n-grams, Understanding LLM-based extraction.

Concept Structure:
<concept id="what-is-a-term">
    <title>What is a Term?</title>
    <!-- Prolog goes here -->
    <conbody>
        <p>A clear definition of a term for extraction purposes...</p>
        <section id="guiding-questions">
            <title>Questions to Define a Term</title>
            <ul>
                <li>Is it domain-specific?</li>
                <li>Is it a single word or a multi-word expression?</li>
            </ul>
        </section>
        <example>
            <title>Example of a Term vs. General Vocabulary</title>
            <p>In software localization, "String" is a term, whereas "Word" is general vocabulary.</p>
        </example>
    </conbody>
    <related-links>
        <!-- Standardized way to include references -->
        <link href="references/cabre_terminology_theory.dita" format="dita"/>
    </related-links>
</concept>
B. Reference Topics (The Look-up Information)

Used for factual, structured data that learners need to look up but not necessarily read top-to-bottom.

    Examples from your course: List of Morphological Patterns, Comparison of LLM Algorithms, Supported formats in MultiTerm Extract.

    Structure: Relies heavily on <refbody>, <properties>, and <table> to list tool features or regex/n-gram patterns.

C. Task Topics (The "How-To")

Used when you are teaching the learner how to execute a specific extraction method.

    Examples from your course: How to clean text down to a manageable list, Analyzing text with AntConc, Writing an LLM prompt for extraction.

    Structure: Uses <taskbody>, <context>, <steps>, and <step>.

D. Glossary (<glossentry>)

A mini-course on terminology needs a glossary. DITA has built-in glossary topics.

    Examples: Definitions for AntConc, n-gram, Chunk Phrase.

3. The Map Structure (The Course Builder)

To assemble these topics into a mini-course, you will use a DITA <map>. Specifically, you can use the Learning Map (<learningMap>) which allows you to define modules, objectives, and assessments.

Here is what your Map Content Model would look like for the Terminology Extraction course:

Course Map (term-extraction-minicourse.ditamap)

    learningOverview: Course Introduction, Prerequisites, and Learning Objectives.

    learningGroup (Module 1): Preparation

        learningContent: What is a term? (Concept)

        learningContent: Questions to ask for term definition (Concept)

    learningGroup (Module 2): Extraction Patterns

        learningContent: Understanding n-grams (Concept)

        learningContent: Morphological patterns (Concept)

            Nested Reference: Table of common morphological patterns (Reference)

    learningGroup (Module 3): Categories of Extraction

        Sub-module 3.1: Manual Extraction

            learningContent: Read & Highlight method (Concept + Task)

            learningContent: Analyzing with AntConc (Task)

            learningContent: Cleaning text (Task)

        Sub-module 3.2: Ready-Made Tools

            learningContent: MultiTerm Extract (Task/Concept)

            learningContent: Sketch Engine (Task/Concept)

        Sub-module 3.3: NLP-based Extraction

            learningContent: Keywords vs. Chunk Phrases (Concept)

            learningContent: Standard Algorithms (Reference)

        Sub-module 3.4: LLM-based Extraction

            learningContent: Prompting LLMs for extraction (Concept + Task)

    learningSummary: Course Wrap-up.

4. Best Practices for Your Knowledgebase Architecture

To make sure your content scales as you build more mini-courses, establish these rules in your content model:

    Strict Separation of Tool vs. Concept:
    Write "Understanding n-grams" in a completely separate file from "Finding n-grams in AntConc". This allows you to reuse the "n-grams" concept in a future course about Python NLP, without bringing AntConc baggage with it.

    Use Content References (@conref):
    If you have a standard warning (e.g., "Note: Always clean your corpus of HTML tags before extraction"), write it once in a reusables.dita file and <note conref="reusables.dita#notes/clean-corpus"/> everywhere else.

    Use Keys (@keyref) for Tool Names and Links:
    Instead of typing "MultiTerm Extract" 50 times, define a key in your map: <keydef keys="tool-multiterm"> <topicmeta><keywords><keyword>MultiTerm Extract</keyword></keywords></topicmeta> </keydef>. In your text, write <keyword keyref="tool-multiterm"/>. If the software changes its name (e.g., to Trados Terminology), you change it in one place.

    Relationship Tables (reltable):
    Instead of hardcoding "See also" links at the bottom of your topics, use a reltable in your map to link "Manual Extraction" to "AntConc". This ensures links never break if you reuse a topic in a course that doesn't include AntConc.
Option 1: The "Docs as Code" Approach (File Structure + Git)

Best for: Solo creators, small teams, and startups building mini-courses.

In this approach, your knowledgebase lives in a standard folder structure on your computer, and you use Git (via GitHub, GitLab, or Bitbucket) as your "database" to store versions, track changes, and back up your work.

Because DITA files are just text (.dita and .ditamap), Git handles them beautifully. You write using an editor (like Oxygen XML Editor, XML Notepad, or VS Code with DITA extensions), and push your files to a Git repository.

What the file structure looks like:
If you go this route, you shouldn't just dump all files in one folder. A scalable file structure looks like this:

📁 my-dita-knowledgebase/
│
├── 📁 shared-resources/        # Things used across all courses
│   ├── 📁 images/              # Logos, generic diagrams
│   ├── 📁 reusables/           # Warnings, tips, snippets (using @conref)
│   ├── 📁 glossaries/          # Master glossary entries
│   └── master-keys.ditamap     # Defines product names, external URLs
│
├── 📁 topics/                  # Your actual content, sorted by domain
│   ├── 📁 nlp-basics/          
│   │   ├── c_what_is_an_ngram.dita
│   │   ├── c_chunk_phrases.dita
│   │   └── r_algorithm_types.dita
│   ├── 📁 term-extraction/
│   │   ├── c_defining_a_term.dita
│   │   ├── t_clean_corpus.dita
│   │   └── t_use_antconc.dita
│
└── 📁 courses/                 # The maps that pull the topics together
    ├── course-term-extraction.ditamap
    └── course-intro-to-nlp.ditamap

    Pros: 100% Free (if using GitHub/GitLab). Complete ownership of your files. Extremely portable (you aren't locked into a vendor's software).

    Cons: If you rename or move a file, you have to ensure you update any links pointing to it (though good XML editors help automate this).
Option 2: Component Content Management System (CCMS)

Best for: Mid-to-large enterprises, huge localization budgets, teams of 5+ writers.

A CCMS is a specialized database built specifically for DITA. In a CCMS, you don't really deal with folders. The software stores every DITA topic, map, and image as an object in a database.

Examples of DITA CCMSs include Heretto, Oxygen Content Fusion, IXIASOFT, or Tridion Docs.

    Pros:

        Bulletproof links: Because topics are database objects, you can rename them or move them around in the UI, and links never break.

        Where-used analytics: You can click on the concept "What is an n-gram" and the database will tell you: "This topic is currently used in the Term Extraction course and the Intro to NLP course."

        Translation: You can send only the updated paragraphs to translators, saving thousands of dollars.

    Cons: Very expensive. They often start at hundreds or thousands of dollars a month. They have a steep learning curve.

Option 3: XML Database (Build-Your-Own)

Best for: Developers building custom web apps to display the courses.

If you are a programmer, you can store DITA files natively in an XML Database (like eXist-db or MarkLogic). This allows you to write XQuery or XPath scripts to query your content. For example, you could write a query to fetch a list of all <concept> topics that contain an <example> element and display them dynamically on a website.
My Recommendation for Your Mini-Courses

Start with Option 1 (File Structure + Git).

Since you are building a knowledgebase for mini-courses, a well-organized folder structure combined with a professional XML editor (like Oxygen XML Editor or Oxygen XML Author) will give you 90% of the benefits of a database without the massive cost.

Oxygen has built-in features (like the "DITA Maps Manager") that act sort of like a mini-database interface—it manages your links, shows you where components are reused, and handles publishing your files into PDFs, HTML5, or SCORM packages for Learning Management Systems (LMS).

How do you plan to eventually publish or deliver these mini-courses to your learners? (e.g., as a website, a PDF handbook, or uploaded to a platform like Teachable/Moodle?) This will also dictate how your files should be organized.

-- It would be a website in my content stack: blog article about term extraction, link to a presentation with an overview of the concepts and link to the minicourse as html, free but with link to video course, if Ihave one, on a platform

This is a fantastic content marketing funnel. What makes your strategy perfect for DITA is Single-Source Publishing.

Instead of writing a blog post in WordPress, building a presentation in PowerPoint, and coding a mini-course in HTML, you can write the content once in your DITA file structure and use the DITA Open Toolkit (DITA-OT) to automatically generate all these different formats.

Here is how your DITA knowledgebase feeds directly into your specific content stack:
1. The Mini-Course (HTML)

This is your core output. Using a publishing engine like the DITA-OT (which is free and open-source) or a tool like Oxygen XML WebHelp, you can transform your course-term-extraction.ditamap directly into a fully functional HTML5 website.

    How it works: DITA automatically generates the table of contents (navigation sidebar), next/previous buttons, and responsive HTML pages based on your map structure.

    Result: A professional, standalone HTML site that you can host for free on GitHub Pages, Netlify, or your own web server.

2. The Presentation (Overview of Concepts)

You don't need to rewrite your concepts for the presentation. Because DITA is modular, you can create a second map—a "Presentation Map"—that pulls in just the top-level concept topics from your mini-course (e.g., What is a Term?, Understanding n-grams).

    How it works: There are open-source plugins for the DITA-OT (like the DITA-to-Reveal.js plugin) that convert DITA topics directly into HTML5 slide decks.

    Result: Every <concept> becomes a slide title. Every <ul> becomes bullet points on the slide. Every <example> becomes a slide visual. If you update the definition of "n-gram" in your knowledgebase, both the mini-course and the presentation update automatically.

3. The Blog Article (Top of Funnel)

Your blog article is essentially the introduction to your mini-course.

    How it works: You can take the <learningOverview> topic and the first <concept> topic and run a quick DITA-to-Markdown or DITA-to-HTML transformation.

    Result: You get perfectly formatted text to paste directly into WordPress, Ghost, or whatever blog platform you use, ensuring your terminology remains 100% consistent across all mediums.

4. The Upsell: Linking to the Video Course

You mentioned linking to a paid video course if you have one. DITA handles this beautifully through a feature called Conditional Processing (Profiling).

You can add your promotional links directly into your DITA files and tag them with an attribute, like deliveryTarget="upsell".

Example in your DITA file:
<conbody>
    <p>Using algorithms to extract chunk phrases requires a basic understanding of POS (Part of Speech) tagging.</p>
    
    <!-- This note only appears in the free HTML mini-course -->
    <note type="tip" deliveryTarget="free-course">
        Want to see this in action? 
        <xref href="https://yourplatform.com/video-course" format="html" scope="external">
            Join the full Video Masterclass on Terminology Extraction!
        </xref>
    </note>
</conbody>
When you generate the free HTML mini-course, you tell the publishing engine to include elements tagged free-course. If you later generate a PDF for paid students, you tell the engine to exclude those promotional links so paying students don't get advertised to.
How to set this up technically

Since you are aiming for a modern web stack, here is the standard pipeline you would build:

    Write: Author your topics in an editor (like Oxygen XML Editor or VS Code) using the Option 1 folder structure we discussed.

    Assemble: Create the specific maps (minicourse.ditamap and presentation.ditamap).

    Publish (The Build Step): Run the DITA Open Toolkit. It acts like a compiler. You feed it your DITA XML, tell it what format you want (HTML5, Reveal.js slides, PDF, Markdown), and it spits out the finished files.

    Deploy: Upload the resulting HTML folder to your web host.

Next Step for You:
Since you know what you are building and how you will deliver it, the very next step is creating your first actual DITA file to get a feel for the markup.
Here is the actual DITA XML code to kickstart your knowledgebase.

This gives you two files: the Learning Map (which acts as the "table of contents" and course builder) and your first Concept Topic (the actual content).
1. The Course Map (course-term-extraction.ditamap)

This file lives in your courses/ folder. It uses the DITA Learning & Training specialization to structure the content into modules (<learningGroup>) and lessons (<learningObject>).

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE learningMap PUBLIC "-//OASIS//DTD DITA Learning Map//EN" "learningMap.dtd">
<learningMap id="course-term-extraction">
    
    <title>Terminology Extraction for Technical Translators</title>
    
    <!-- Course-level metadata -->
    <topicmeta>
        <author type="creator">Your Name</author>
        <audience type="user" job="translating" experiencelevel="intermediate"/>
    </topicmeta>

    <!-- MODULE 1: Preparation -->
    <learningGroup id="mod-preparation">
        <topicmeta>
            <navtitle>Preparation &amp; Definitions</navtitle>
        </topicmeta>
        <learningObject>
            <!-- Here we pull in your actual concept file -->
            <learningContentRef href="../topics/term-extraction/c_what_is_a_term.dita" format="dita"/>
            <!-- Future file: <learningContentRef href="../topics/term-extraction/c_definition_questions.dita"/> -->
        </learningObject>
    </learningGroup>

    <!-- MODULE 2: Extraction Patterns -->
    <learningGroup id="mod-patterns">
        <topicmeta>
            <navtitle>Patterns to Extract</navtitle>
        </topicmeta>
        <learningObject>
            <!-- Future files -->
            <!-- <learningContentRef href="../topics/nlp-basics/c_understanding_ngrams.dita"/> -->
            <!-- <learningContentRef href="../topics/nlp-basics/c_morphological_patterns.dita"/> -->
        </learningObject>
    </learningGroup>

    <!-- MODULE 3: Categories of Extraction -->
    <learningGroup id="mod-extraction-methods">
        <topicmeta>
            <navtitle>Extraction Methods &amp; Tools</navtitle>
        </topicmeta>
        
        <!-- Sub-module 3.1 -->
        <learningObject id="manual-extraction">
            <topicmeta><navtitle>Manual Extraction</navtitle></topicmeta>
            <!-- <learningContentRef href="../topics/term-extraction/t_read_and_highlight.dita"/> -->
            <!-- <learningContentRef href="../topics/term-extraction/t_analyze_antconc.dita"/> -->
        </learningObject>
        
        <!-- Sub-module 3.2 -->
        <learningObject id="ready-made-tools">
            <topicmeta><navtitle>Ready-Made Tools</navtitle></topicmeta>
            <!-- <learningContentRef href="../topics/term-extraction/c_multiterm_extract.dita"/> -->
        </learningObject>
        
        <!-- Sub-module 3.3 -->
        <learningObject id="nlp-llm-tools">
            <topicmeta><navtitle>NLP &amp; LLM-Based Extraction</navtitle></topicmeta>
            <!-- <learningContentRef href="../topics/nlp-basics/c_keywords_vs_chunks.dita"/> -->
            <!-- <learningContentRef href="../topics/term-extraction/t_llm_prompting.dita"/> -->
        </learningObject>
    </learningGroup>

</learningMap>

2. The Concept Topic (c_what_is_a_term.dita)

This file lives in your topics/term-extraction/ folder. Notice how it strictly focuses on the concept of what a term is, making it highly reusable. It also includes the upsell link we discussed using conditional tagging.

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE concept PUBLIC "-//OASIS//DTD DITA Concept//EN" "concept.dtd">
<concept id="what_is_a_term">
    
    <title>What is a Term?</title>
    
    <!-- Topic-level metadata for searching and filtering -->
    <prolog>
        <author type="creator">Your Name</author>
        <critdates>
            <created date="2026-02-23"/>
        </critdates>
        <metadata>
            <category>Terminology</category>
            <category>Theory</category>
        </metadata>
    </prolog>

    <conbody>
        <!-- A short description or summary of the concept -->
        <p>Before you can extract terminology from a text, you must first define what qualifies as a "term" within your specific domain. A term is not simply any word; it is a linguistic representation of a specialized concept.</p>
        
        <section id="characteristics">
            <title>Core Characteristics of a Term</title>
            <p>For the purposes of technical translation and extraction, a viable term usually possesses the following traits:</p>
            <ul>
                <li><b>Domain-specific:</b> It has a precise meaning in a specific field (e.g., engineering, software) that may differ from its general use.</li>
                <li><b>Monoreferential:</b> In its specific context, it refers to exactly one concept.</li>
                <li><b>Multi-word or Single-word:</b> It can be a single noun or a complex noun phrase (n-gram).</li>
            </ul>
        </section>

        <!-- The required Example element from your Content Model rules -->
        <example id="term-example">
            <title>Example: General Vocabulary vs. Term</title>
            <p>Consider the word <i>"String"</i>.</p>
            <ul>
                <li><b>General vocabulary (Textile domain):</b> A piece of long, thin material used for tying.</li>
                <li><b>Term (Software domain):</b> A sequence of characters treated as a single data type.</li>
            </ul>
            <p>When extracting terms for software localization, "String" must be captured as a specialized term.</p>
        </example>

        <!-- Conditional text: The Upsell link that only shows in the free HTML version -->
        <note type="tip" deliveryTarget="free-course">
            Want to dive deeper into the linguistic theory behind terminology? 
            <xref href="https://yourwebsite.com/video-course" format="html" scope="external">
                Check out the full Video Masterclass!
            </xref>
        </note>
    </conbody>
    
    <!-- References to standard definitions or external readings -->
    <related-links>
        <link href="https://www.iso.org/obp/ui/#iso:std:iso:1087:ed-2:v1:en" format="html" scope="external">
            <linktext>ISO 1087:2019 - Terminology work and terminology science</linktext>
        </link>
    </related-links>

</concept>

How to use these files right now:

    Create a main folder on your computer called my-dita-kb.

    Inside it, create two folders: courses and topics.

    Inside topics, create a folder called term-extraction.

    Copy the Map XML and save it as course-term-extraction.ditamap inside the courses folder.

    Copy the Concept XML and save it as c_what_is_a_term.dita inside the topics/term-extraction folder.

If you open that Map file in a DITA editor (like the free 30-day trial of Oxygen XML Editor), it will automatically resolve the link, show you the hierarchy, and allow you to click a single "Transform" button to turn it into an HTML5 website instantly.

## Setup course section / basic dev skills

By building this "Setup / Basic Dev Skills" section as a standalone module (a sub-map), you can simply drop <mapref href="shared-prereqs/dev-setup.ditamap"/> into any future technical course you create. You write it once, and it serves as the prerequisite for everything else.

Your pedagogical approach—translating developer concepts into the familiar paradigm of software UIs for translators—is exactly how this audience needs to be taught.

Here is the analytical breakdown and the DITA-optimized outline for this 20-minute setup module.
Analysis of the Requirements & DITA Mapping

    Mindset Shift (Concept): Translators know CAT tools (Trados, MemoQ). We need a Concept topic that maps "Code" to "Software Interface" (Libraries = Menus, Functions = Buttons).

    OS Profiling (DITA Magic): Because your audience is heavily Windows-based, we will use DITA's @platform attribute. In your tasks, you can write steps for Windows, Mac, and Linux, but tag them <step platform="windows">. When you publish, you can either show them as tabs, or generate a "Windows-only" version of the course.

    The Editor (Concept + Tasks): We introduce VS Code, install it, get the Python extension, and pull the practice files. Git is treated purely as a "download tool" (no theory).

    The Environment (Concept + Tasks): Terminals are scary. Virtual environments are confusing. We need a Concept topic using a "Sandboxes" or "Separate computer profiles" analogy, followed by the specific Miniconda setup.

The DITA Content Model Outline

Here is the structured outline for "Most Basic Dev Skills for Translators in 20 Min".

Notice how strict the separation is between "Understanding" (C) and "Doing" (T).

    Module Map: dev-setup-translators.ditamap

        Lesson 1: Changing How You See Code

            C_code_as_software.dita (Concept: Code is just a UI without the graphics. Python = Base OS. Libraries = Dropdown Menus. Functions = Clicks/Buttons.)

        Lesson 2: Setting Up Your Workspace (VS Code)

            C_editors_vs_terminals.dita (Concept: Why we don't just type into a black box. What VS Code actually does.)

            T_install_vscode.dita (Task: Downloading and installing VS Code. Includes OS-specific notes for Windows/Mac).

            T_install_python_extension.dita (Task: Getting the Python and Jupyter extensions inside VS Code so it understands Python syntax.)

        Lesson 3: Getting the Practice Files

            T_setup_local_folder.dita (Task: Creating my-learnings, opening it in VS Code, and opening the integrated terminal.)

            T_clone_repo.dita (Task: Typing git clone [link] in the terminal. Purely framed as a "secure download method" rather than version control.)

        Lesson 4: The Engine & Virtual Environments

            C_package_managers_and_terminals.dita (Concept: Demystifying the terminal. Why developers install things via text instead of .exe files. What a package manager is.)

            C_why_virtual_environments.dita (Concept: The "Sandbox" analogy. Why we don't install NLP tools directly to your main Windows system to prevent breaking things.)

        Lesson 5: Installing Python & NLP Tools (Miniconda)

            T_install_miniconda.dita (Task: Downloading and installing Miniconda3. Crucial Windows step: Explaining whether to add to PATH or use the Anaconda Prompt.)

            T_create_activate_env.dita (Task: Creating the environment with Python 3.12: conda create -n nlp-course python=3.12, and activating it.)

            T_install_nlp_packages.dita (Task: Using pip or conda to install spacy, nltk, and pandas into the activated environment.)

The DITA Map Code (dev-setup-translators.ditamap)

Here is the exact code you would use to build this reusable module. You can save this in a folder called shared-prereqs/ and reuse it endlessly.

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE map PUBLIC "-//OASIS//DTD DITA Map//EN" "map.dtd">
<map id="dev-setup-translators">
    
    <title>Prerequisite: Most Basic Dev Skills for Translators in 20 Min</title>
    
    <topicmeta>
        <author>Your Name</author>
        <shortdesc>A 20-minute rapid setup guide bridging the gap between translator software and Python development environments.</shortdesc>
    </topicmeta>

    <!-- LESSON 1: Mental Model -->
    <topicgroup>
        <topicmeta><navtitle>1. Changing How You See Code</navtitle></topicmeta>
        <topicref href="topics/dev-setup/c_code_as_software.dita" type="concept"/>
    </topicgroup>

    <!-- LESSON 2: The Workspace -->
    <topicgroup>
        <topicmeta><navtitle>2. Setting Up Your Workspace (VS Code)</navtitle></topicmeta>
        <topicref href="topics/dev-setup/c_editors_vs_terminals.dita" type="concept"/>
        <topicref href="topics/dev-setup/t_install_vscode.dita" type="task"/>
        <topicref href="topics/dev-setup/t_install_python_extension.dita" type="task"/>
    </topicgroup>

    <!-- LESSON 3: Getting Files -->
    <topicgroup>
        <topicmeta><navtitle>3. Getting the Practice Files</navtitle></topicmeta>
        <topicref href="topics/dev-setup/t_setup_local_folder.dita" type="task"/>
        <topicref href="topics/dev-setup/t_clone_repo.dita" type="task"/>
    </topicgroup>

    <!-- LESSON 4: Environments (The Scary Stuff Demystified) -->
    <topicgroup>
        <topicmeta><navtitle>4. Package Managers &amp; Environments</navtitle></topicmeta>
        <topicref href="topics/dev-setup/c_package_managers_and_terminals.dita" type="concept"/>
        <topicref href="topics/dev-setup/c_why_virtual_environments.dita" type="concept"/>
    </topicgroup>

    <!-- LESSON 5: Installation Tasks -->
    <topicgroup>
        <topicmeta><navtitle>5. Installing Python &amp; NLP Tools</navtitle></topicmeta>
        <topicref href="topics/dev-setup/t_install_miniconda.dita" type="task"/>
        <topicref href="topics/dev-setup/t_create_activate_env.dita" type="task"/>
        <topicref href="topics/dev-setup/t_install_nlp_packages.dita" type="task"/>
    </topicgroup>

</map>

Pro-Tip for Writing Tasks for this Audience

When you write the <task> files (like t_clone_repo.dita), DITA gives you a specific element called <stepxmp> (Step Example) and <info> (Information).

Since this audience is anxious about code, use the <info> tag to explain why they are doing a step, rather than just telling them to type it.

Example of a step in DITA for this audience:

<step>
    <cmd>In the VS Code terminal window, type the following command and press <uicontrol>Enter</uicontrol>:</cmd>
    <stepxmp>
        <codeblock>git clone https://github.com/yourusername/nlp-practice-files.git</codeblock>
    </stepxmp>
    <info>
        <note type="note">Think of <userinput>git clone</userinput> as an automated "Download ZIP" button. It safely copies the course folder from the internet directly into your local <filepath>my-learnings</filepath> folder.</note>
    </info>
</step>

### Troubleshooting

Adding a troubleshooting section and introducing GitHub Copilot is the perfect way to round out this module. It moves the learner from "just following steps" to actually feeling empowered to solve problems.

To answer your question first: No, GitHub Copilot is not activated automatically in VS Code.

However, the great news for your 2026 learners is that GitHub now offers a Free tier of Copilot built right into VS Code (giving 2,000 code completions and 50 chat requests per month, which is plenty for a beginner translator). They just need a GitHub account, the extension, and to sign in.

Here is how we weave Troubleshooting and AI Assistance into your DITA content model, using translator-friendly analogies.
1. The Updated DITA Outline

We will add two new lessons to the end of your dev-setup-translators.ditamap.

    Lesson 6: Troubleshooting (Don't Panic)

        C_reading_error_messages.dita (Concept: Translators are used to weird XML/Tag errors in CAT tools. Python errors are the same: just read the bottom line. Is the file saved? Is the environment activated?)

        R_common_setup_errors.dita (Reference: A quick troubleshooting table. Problem -> Solution. E.g., "Command not found" = You probably didn't activate the virtual environment.)

    Lesson 7: Your AI Co-Translator (GitHub Copilot)

        C_what_is_copilot.dita (Concept: The Analogy. Copilot is exactly like AutoSuggest / Machine Translation, but for code. It guesses your next word.)

        T_activate_copilot_free.dita (Task: Installing the extension, signing into GitHub, and activating the Free tier.)

        T_using_copilot_chat.dita (Task: How to open the Chat panel, and crucially, how to type @workspace so the AI knows to look at the specific practice files they just cloned.)

2. The Map Additions (XML)

You would append this code to the bottom of your existing dev-setup-translators.ditamap, right before the closing </map> tag:

<!-- LESSON 6: Troubleshooting -->
    <topicgroup>
        <topicmeta><navtitle>6. Troubleshooting: When Things Turn Red</navtitle></topicmeta>
        <topicref href="topics/dev-setup/c_reading_error_messages.dita" type="concept"/>
        <topicref href="topics/dev-setup/r_common_setup_errors.dita" type="reference"/>
    </topicgroup>

    <!-- LESSON 7: GitHub Copilot -->
    <topicgroup>
        <topicmeta><navtitle>7. Activating GitHub Copilot</navtitle></topicmeta>
        <topicref href="topics/dev-setup/c_what_is_copilot.dita" type="concept"/>
        <topicref href="topics/dev-setup/t_activate_copilot_free.dita" type="task"/>
        <topicref href="topics/dev-setup/t_using_copilot_chat.dita" type="task"/>
    </topicgroup>

3. Example Content: The Copilot Concept Topic

Here is how you would write the Copilot concept (c_what_is_copilot.dita) using the translator-analogy approach. This is the exact kind of framing that lowers anxiety for non-developers.

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE concept PUBLIC "-//OASIS//DTD DITA Concept//EN" "concept.dtd">
<concept id="what_is_copilot">
    <title>GitHub Copilot: Machine Translation for Code</title>
    
    <conbody>
        <p>As a translator, you are likely used to CAT tools offering <b>AutoSuggest</b> or populating a segment with <b>Machine Translation (MT)</b>. GitHub Copilot does exactly the same thing, but for programming languages.</p>
        
        <p>It is an AI assistant built directly into VS Code. It is not activated automatically; you must install it and sign in with your GitHub account to use the Free tier.</p>

        <section id="how-it-helps">
            <title>How it works in your editor</title>
            <p>Copilot helps you in two distinct ways:</p>
            <ul>
                <li><b>Ghost Text (AutoSuggest):</b> As you type Python code, Copilot will suggest the rest of the line in faint gray text. If you like the suggestion, you press <uicontrol>Tab</uicontrol> to accept it.</li>
                <li><b>Copilot Chat (Your Dev Support):</b> You can open a chat sidebar in VS Code and ask questions in plain English, such as <i>"How do I run this specific Python file?"</i></li>
            </ul>
        </section>

        <section id="the-context-rule">
            <title>The Golden Rule: Context is Everything</title>
            <p>Just like an MT engine produces better translations if it has context, Copilot gives better answers if it knows what folder you are working in.</p>
            <p>Because you opened your <filepath>my-learnings</filepath> folder in VS Code, you can use the special command <b>@workspace</b> in the chat. This tells the AI: <i>"Look at all the files in this specific folder before answering my question."</i></p>
        </section>
        
        <example>
            <title>Example Chat Prompt</title>
            <p>Instead of asking: <i>"Why is my code broken?"</i></p>
            <p>Ask: <i>"<b>@workspace</b> I am trying to run the term extraction script, but I am getting a 'pandas not found' error. How do I fix this?"</i></p>
        </example>
    </conbody>
</concept>

4. Example Content: The Troubleshooting Reference Table

For r_common_setup_errors.dita, DITA's <properties> table or a simple <table> is perfect for mapping symptoms to cures.

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE reference PUBLIC "-//OASIS//DTD DITA Reference//EN" "reference.dtd">
<reference id="common_setup_errors">
    <title>Common Setup Errors</title>
    <refbody>
        <section>
            <title>Quick Fixes</title>
            <p>If you hit an error in the terminal, check this table before asking Copilot.</p>
            
            <table frame="all">
                <tgroup cols="3">
                    <colspec colname="c1" colwidth="1*"/>
                    <colspec colname="c2" colwidth="1*"/>
                    <colspec colname="c3" colwidth="2*"/>
                    <thead>
                        <row>
                            <entry>The Symptom</entry>
                            <entry>The Cause</entry>
                            <entry>The Fix</entry>
                        </row>
                    </thead>
                    <tbody>
                        <row>
                            <entry>Terminal says <code>conda: command not found</code></entry>
                            <entry>Miniconda isn't in your PATH, or you restarted VS Code.</entry>
                            <entry>On Windows, close the normal terminal and open the <b>Anaconda Prompt</b> from your Start Menu, OR ensure you checked "Add to PATH" during installation.</entry>
                        </row>
                        <row>
                            <entry>Terminal says <code>ModuleNotFoundError: No module named 'spacy'</code></entry>
                            <entry>You forgot to activate your virtual environment.</entry>
                            <entry>Run <code>conda activate nlp-course</code>. You should see <b>(nlp-course)</b> at the start of your terminal line.</entry>
                        </row>
                        <row>
                            <entry>A white dot appears next to your file name at the top of VS Code.</entry>
                            <entry>Unsaved changes.</entry>
                            <entry>Press <uicontrol>Ctrl + S</uicontrol> (Windows) or <uicontrol>Cmd + S</uicontrol> (Mac). Python cannot run code you haven't saved!</entry>
                        </row>
                    </tbody>
                </tgroup>
            </table>
        </section>
    </refbody>
</reference>

By adding these files, your learners get a complete, closed-loop onboarding experience. If they get stuck, Lesson 6 catches them. If they want to push further, Lesson 7 gives them the AI training wheels to do so.