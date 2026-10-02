# TASK: Build a Complete Local Bangla PDF → Markdown Processing System

You are operating directly on my Windows PC and have permission to inspect the machine, create files/folders, run commands, install software, configure environments, download dependencies when possible, and test the complete system.

Your job is to **research, design, install, configure, benchmark, test, document, and finalize a complete local PDF-to-Markdown processing pipeline**.

The primary purpose of this system is:

> Convert large Bangla educational PDF books, especially Bangladesh NCTB textbooks, into high-quality Markdown that can later be consumed by a local LLM/RAG system for question answering, summarization, search, and educational use.

The system must prioritize:

1. Bangla/Bengali OCR quality
2. Large PDFs, including approximately 300–400 pages
3. Preservation of document structure
4. Headings and sections
5. Reading order
6. Tables
7. Lists
8. Page/section boundaries where useful
9. Mathematical/scientific content where possible
10. Images/figures and their surrounding context
11. Clean Markdown suitable for LLM/RAG
12. Fully local/offline operation after installation
13. Zero or near-zero recurring cost
14. Reliable cleanup of GPU/RAM/process resources
15. Easy operation by a non-technical user after setup
16. Configurable performance profiles
17. Reproducibility and maintainability

---

# 1. IMPORTANT: DO NOT START INSTALLING RANDOMLY

Before changing anything, perform a proper technical investigation.

Inspect the actual PC and determine:

* Windows version
* CPU
* physical RAM
* GPU(s)
* GPU VRAM
* NVIDIA driver version if applicable
* CUDA availability/version
* available disk space
* available SSD/HDD locations
* Python version(s)
* Git availability
* PowerShell version
* Visual C++ runtime/build tools if relevant
* Docker availability if relevant
* existing OCR/PDF tools
* existing Python environments
* existing AI/LLM-related installations
* whether another process is already using the GPU
* whether WSL is installed
* whether any existing software conflicts with the planned installation

Do not unnecessarily modify existing AI installations.

The target installation must be isolated inside the folder that I provide.

I will provide the installation root, for example:

`C:\AI\BanglaPDF`

Treat the path I provide as:

`<INSTALL_ROOT>`

Do not hard-code the example path.

---

# 2. RESEARCH THE CURRENT BEST OPEN-SOURCE TECHNOLOGY

Before selecting the final architecture, research the currently available open-source tools.

At minimum investigate:

* Microsoft MarkItDown
* Marker
* Surya OCR
* Surya 2 if applicable
* PaddleOCR / PaddleOCR-VL
* other strong Bengali OCR/document-parsing solutions
* Bengali-specific OCR projects where relevant
* PDF text extraction tools
* layout analysis tools
* table extraction tools
* Markdown/document conversion tools

Do not blindly follow this list.

The goal is to determine the best practical architecture for:

> Bangla NCTB scanned/text PDFs → high-quality structured Markdown → LLM/RAG

Check current official GitHub repositories and documentation where possible.

For each serious candidate, investigate:

* Bengali/Bangla support
* OCR quality
* scanned PDF support
* selectable-text PDF support
* layout preservation
* reading-order detection
* table handling
* mathematical notation
* images
* multi-column documents
* headings
* lists
* footnotes
* page numbering
* large-document behavior
* 300–400 page PDFs
* GPU requirements
* VRAM requirements
* RAM requirements
* Windows compatibility
* offline capability
* license
* installation complexity
* maintenance status
* model download size
* speed
* known problems
* Markdown output quality

Do not select a tool merely because it is popular.

Select the architecture based on evidence and the actual machine.

If another tool becomes clearly better than Marker + Surya, use that instead.

---

# 3. BENGALI/NCTB IS THE PRIMARY REQUIREMENT

This is NOT a generic English PDF converter.

The system must be optimized for Bangla educational textbooks.

Assume that NCTB PDFs may contain:

* Bangla Unicode text
* scanned Bangla pages
* selectable Bangla text
* Bangla headings
* Bangla paragraphs
* English mixed with Bangla
* numbers
* mathematical equations
* tables
* diagrams
* charts
* illustrations
* captions
* multi-column layouts
* headers and footers
* page numbers
* chapter titles
* exercises
* question/answer sections
* bullet/numbered lists
* unusual textbook typography
* low-quality scans
* skewed pages
* noisy pages
* different font sizes
* colored text
* background graphics

The pipeline should intelligently handle both:

### Type A — Text PDF

If the PDF already contains reliable embedded text, use direct extraction where that produces better results than OCR.

### Type B — Scanned PDF

If pages are images/scans, use Bengali-capable OCR.

### Type C — Mixed PDF

Detect which pages require OCR and process them appropriately.

Do not unnecessarily OCR clean embedded text if doing so would reduce accuracy.

---

# 4. QUALITY OVER RAW SPEED

The primary goal is accurate Markdown for later LLM use.

Do not optimize purely for maximum pages/second.

However, create multiple configurations so that I can choose between:

* maximum quality
* balanced/default
* low resource usage

If the selected OCR engine supports useful parameters for:

* batch size
* GPU usage
* CPU usage
* image resolution
* OCR detection
* layout detection
* table recognition
* parallelism
* worker count

expose them through configuration rather than hard-coding them.

---

# 5. INSTALLATION ARCHITECTURE

Everything belonging to this project should live under:

`<INSTALL_ROOT>`

Create a clean structure similar to:

```text
<INSTALL_ROOT>\
│
├── app\
│   ├── scripts\
│   ├── pipeline\
│   └── utilities\
│
├── models\
│
├── configs\
│   ├── default\
│   ├── high-performance\
│   ├── balanced\
│   ├── low-resource\
│   └── high-quality\
│
├── input\
│
├── output\
│
├── temp\
│
├── logs\
│
├── benchmarks\
│
├── tests\
│
├── reports\
│
├── docs\
│
├── runtime\
│
├── cache\
│
├── start.bat
├── stop.bat
├── config.yaml
└── README.md
```

You may improve this structure if research indicates a better architecture.

Do not pollute unrelated system directories unless absolutely necessary.

Prefer isolated virtual environments or portable environments inside `<INSTALL_ROOT>`.

---

# 6. CONFIGURATION SYSTEM

Create a configuration-driven architecture.

There must be a simple main/default configuration.

For example:

```text
<INSTALL_ROOT>\config.yaml
```

This should be the active configuration.

Also create configuration profiles:

```text
<INSTALL_ROOT>\configs\high-performance\
<INSTALL_ROOT>\configs\balanced\
<INSTALL_ROOT>\configs\low-resource\
<INSTALL_ROOT>\configs\high-quality\
```

Each configuration should contain all relevant settings.

For example:

* OCR engine
* model
* GPU enabled/disabled
* GPU device
* VRAM-related settings
* CPU worker count
* batch size
* image resolution
* OCR language
* layout detection
* table detection
* output format
* image handling
* temporary directory
* output directory
* logging level
* cleanup behavior
* parallel processing
* error recovery
* page processing options

Do not put machine-specific assumptions into application code if they can reasonably live in configuration.

The main `config.yaml` should be easy to replace.

I should be able to do something conceptually like:

```text
copy configs\high-performance\config.yaml config.yaml
```

and then run the application using the new configuration.

Provide a convenient configuration-switching mechanism if practical.

---

# 7. THREE REQUIRED PERFORMANCE PROFILES

After installation, benchmark the system and create at least these profiles.

## A. High Performance

Goal:

Maximum practical throughput while still producing acceptable educational OCR quality.

Use GPU aggressively while leaving enough resources for Windows to remain usable.

## B. Balanced / Default

This must be the default configuration.

Goal:

Good OCR quality + reasonable speed + reasonable system resource usage.

This should be the recommended everyday profile.

## C. Low Resource

Goal:

Allow the PC to remain usable for normal work.

Reduce:

* GPU usage where appropriate
* CPU workers
* batch sizes
* concurrency
* memory consumption

The system should still produce valid Markdown.

Also create:

## D. High Quality

If technically useful, create a profile specifically optimized for maximum document quality, even if it is slower.

---

# 8. START.BAT REQUIREMENTS

Create:

```text
<INSTALL_ROOT>\start.bat
```

The user experience should be extremely simple.

When I double-click `start.bat`, it should:

1. Validate the installation.
2. Validate required models/dependencies.
3. Ask for the PDF path.
4. Ask for an output/destination path.
5. If I leave the destination empty, use the default output directory.
6. Validate the PDF.
7. Display the active configuration.
8. Display important resource information.
9. Start the processing pipeline.
10. Process the entire PDF.
11. Produce Markdown.
12. Save the resulting Markdown in the requested destination.
13. Preserve useful metadata.
14. Save logs.
15. Produce a processing report.
16. Clean temporary files.
17. Shut down worker processes/services.
18. Release GPU resources as much as the underlying framework allows.
19. Exit cleanly.

Example interaction:

```text
========================================
 Bangla PDF → Markdown
========================================

Active configuration: Balanced

Enter PDF path:
>

Enter output directory
Press ENTER for default:
>
```

If the PDF is:

```text
D:\Books\NCTB\Class-9-Bangla.pdf
```

and I leave the destination empty, save to something like:

```text
<INSTALL_ROOT>\output\
```

If I provide:

```text
D:\ConvertedBooks
```

save the output there.

The exact output naming convention should be sensible, e.g.:

```text
Class-9-Bangla.md
```

---

# 9. SUPPORT COMMAND-LINE USAGE TOO

Although the primary interface should be interactive `start.bat`, also make the underlying script usable from the command line.

For example:

```text
start.bat "D:\Books\book.pdf"
```

and:

```text
start.bat "D:\Books\book.pdf" "D:\Output"
```

If practical, support options such as:

```text
--config balanced
--config high-performance
--config low-resource
--config high-quality
```

Do not sacrifice the simple interactive experience.

---

# 10. STOP.BAT REQUIREMENTS

Create:

```text
<INSTALL_ROOT>\stop.bat
```

This is a fallback emergency/cleanup mechanism.

It must stop ONLY processes/services belonging to this PDF processing application.

Do NOT blindly execute:

```text
taskkill /IM python.exe
```

or:

```text
taskkill /IM something.exe
```

if that could terminate unrelated applications.

The stop mechanism must identify processes belonging to this installation using:

* PID files
* process command line
* process ownership
* known working directories
* application-specific markers
* ports, if applicable

Use safe process validation before termination.

`stop.bat` should:

1. Detect running pipeline processes.
2. Confirm they belong to this installation.
3. Stop them gracefully.
4. Wait for termination.
5. Force terminate only if necessary.
6. Clean temporary resources.
7. Remove stale PID/state files.
8. Confirm that application processes are gone.

It must NOT terminate unrelated Python, CUDA, PowerShell, or AI processes.

---

# 11. CLEAN RESOURCE MANAGEMENT

The application must not leave unnecessary processes running after conversion.

At completion:

* close worker processes
* release GPU memory where possible
* close files
* stop background services started by the application
* remove temporary processing files where safe
* remove stale PID files
* flush logs
* return control to Windows

If the OCR framework creates persistent workers, identify how to shut them down correctly.

If a process cannot release GPU memory completely until its process exits, terminate the worker process cleanly rather than leaving it resident.

The fallback `stop.bat` must handle abnormal termination.

---

# 12. LARGE PDF SUPPORT

Explicitly test:

* 10-page PDF
* approximately 50-page PDF
* approximately 100-page PDF
* 300+ page PDF if an appropriate test document is available

Do NOT load an entire huge PDF into RAM unnecessarily.

Prefer streaming/page-batch processing where supported.

The pipeline should be resilient to:

* one problematic page
* corrupted page
* OCR failure
* temporary GPU error
* out-of-memory error
* missing font
* unexpected layout

If one page fails, the whole 400-page job should not necessarily be lost.

Implement sensible checkpointing if supported.

For example:

```text
runtime\jobs\<job-id>\
```

Store enough information to resume or diagnose a failed job.

---

# 13. OUTPUT MARKDOWN DESIGN

The resulting Markdown must be optimized for LLM/RAG use rather than visual reproduction.

Prefer clean structure such as:

```markdown
# Chapter 1: ...

## 1.1 ...

Bangla paragraph...

### উদাহরণ

...

## অনুশীলনী

1. ...
2. ...

## গুরুত্বপূর্ণ তথ্য

...
```

Do not unnecessarily reproduce:

* repeated headers
* repeated footers
* page numbers
* OCR garbage
* decorative elements

when they can be reliably identified.

However, do not aggressively delete text when uncertain.

Preserve meaningful educational content.

---

# 14. PAGE INFORMATION

If useful, preserve page boundaries using comments or headings, for example:

```markdown
<!-- Page 27 -->
```

or another clean mechanism.

Do not clutter the final Markdown unnecessarily.

The configuration should determine whether page markers are included.

---

# 15. IMAGES AND FIGURES

Determine the best approach for figures.

If the OCR/document parser extracts images:

Store them in a related directory:

```text
output\
    book.md
    book_assets\
        image_001.png
        image_002.png
```

and reference them from Markdown when useful.

Do not embed enormous images unnecessarily.

If an image contains important educational text, investigate whether OCR can process it.

---

# 16. TABLES

Tables are important.

Research the selected parser's table extraction capabilities.

Test Bangla tables and mixed Bangla/English tables where possible.

Prefer:

```markdown
| বিষয় | বিবরণ |
|---|---|
| ... | ... |
```

when the source structure permits.

If a table cannot be represented reliably, preserve its information in a structured fallback rather than silently losing it.

---

# 17. MATHEMATICS AND SCIENCE

NCTB textbooks can contain:

* equations
* chemical formulas
* mathematical symbols
* units
* superscripts/subscripts
* diagrams

Research the selected tools' capabilities.

Where possible, preserve equations using Markdown-compatible LaTeX:

```markdown
$$
E = mc^2
$$
```

Do not invent mathematical content.

If exact equation recognition is not reliable, preserve the source representation as safely as possible.

Document known limitations.

---

# 18. QUALITY VALIDATION

After setup, create an automated test suite.

Test at minimum:

### Test 1 — Bangla Unicode

Verify that Bengali characters are correctly preserved.

### Test 2 — English + Bangla

Verify mixed-language pages.

### Test 3 — Long document

Test a large PDF.

### Test 4 — Scanned PDF

Verify OCR.

### Test 5 — Text PDF

Verify direct extraction.

### Test 6 — Table

Verify table extraction.

### Test 7 — Multi-column

Verify reading order.

### Test 8 — Headings

Verify document hierarchy.

### Test 9 — Images

Verify image extraction/reference handling.

### Test 10 — Resource cleanup

Verify no unwanted pipeline processes remain after completion.

---

# 19. BENCHMARKING

After setup, perform benchmarks using representative documents.

Measure:

* total processing time
* pages/minute
* pages/second if useful
* peak RAM
* peak VRAM
* average VRAM
* CPU utilization
* GPU utilization
* output Markdown size
* number of processed pages
* number of failed pages
* OCR errors detected through basic automated checks
* processing errors
* cleanup success

Create a report:

```text
<INSTALL_ROOT>\benchmarks\
```

For example:

```text
benchmark-balanced.txt
benchmark-high-performance.txt
benchmark-low-resource.txt
benchmark-high-quality.txt
```

Do not claim OCR accuracy unless you have an actual ground-truth comparison.

If we have no ground-truth dataset, clearly label results as performance measurements rather than accuracy measurements.

---

# 20. CONFIGURATION SELECTION

Based on benchmark results, create final configurations.

Do NOT arbitrarily label something "best."

Instead document why each profile exists.

For example:

```text
High Performance
- fastest tested configuration
- higher resource consumption

Balanced
- recommended general configuration
- good balance between quality, speed and resource usage

Low Resource
- lowest resource consumption
- slower processing

High Quality
- maximum quality-oriented configuration
- slower processing
```

The default must be:

```text
Balanced
```

unless testing demonstrates a more appropriate default.

---

# 21. ERROR HANDLING

The application must provide useful errors.

Examples:

```text
ERROR: PDF file does not exist.

ERROR: Required OCR model is missing.

ERROR: Insufficient disk space.

ERROR: GPU is unavailable.

ERROR: PDF could not be opened.

WARNING: This PDF appears to be scanned.

WARNING: OCR will be used and processing may take longer.
```

Do not dump raw Python tracebacks to the user unless debugging is enabled.

Store full technical errors in logs.

---

# 22. LOGGING

Create:

```text
<INSTALL_ROOT>\logs\
```

Every job should have a timestamped log.

Example:

```text
2026-10-02_14-30-00.log
```

The log should record:

* input PDF
* output location
* active configuration
* software versions
* OCR model
* processing start/end time
* page count
* successful pages
* failed pages
* warnings
* resource information
* cleanup status

Do not log sensitive information unnecessarily.

---

# 23. OFFLINE-FIRST DESIGN

After the required models/packages have been downloaded, the actual PDF processing should work without Internet access.

Do not send educational books to external APIs.

Do not use paid cloud OCR.

Do not silently upload PDF contents anywhere.

If a selected tool requires an online service, do not make it the default solution.

Only use an online component if there is no practical local alternative, and clearly ask me first.

---

# 24. DOWNLOADS AND USER INTERACTION

You are allowed to automate downloads where technically and legally appropriate.

However, if something cannot safely/legally/technically be downloaded automatically, STOP and ask me.

Examples:

```text
I need you to download the following official model manually:

<URL>

After downloading it, tell me the exact file path and I will continue.
```

Do not pretend that a dependency was successfully installed if it was not.

Do not proceed based on assumptions.

After I perform the requested action, continue from the current state.

If administrative privileges are required, ask me explicitly before performing the action.

Do not disable Windows security features merely to make installation easier.

---

# 25. LICENSE CHECK

Before installing a major dependency/model, verify its license.

Prefer:

* MIT
* Apache-2.0
* BSD
* similarly permissive licenses

If a model has restrictions, document them.

Do not install questionable or illegally redistributed models.

---

# 26. VERSION PINNING

Record exact versions of important components.

Create something like:

```text
<INSTALL_ROOT>\docs\VERSIONS.md
```

Include:

* Python
* OCR framework
* OCR model
* PDF parser
* Marker/PaddleOCR/etc.
* CUDA-related dependencies
* PyTorch if applicable
* other important packages

If practical, use a requirements file or lock/pinned environment.

The setup should be reproducible.

---

# 27. README

Create:

```text
<INSTALL_ROOT>\README.md
```

It must explain in simple language:

1. What this system does
2. How to use `start.bat`
3. How to use `stop.bat`
4. Where output files are stored
5. How to change configuration
6. Meaning of each profile
7. How to add a new PDF
8. How to troubleshoot
9. How to update the OCR models
10. How to completely uninstall the project
11. Known limitations
12. Benchmark results
13. Installed versions

Include both:

### Simple user instructions

and

### Technical maintenance instructions

---

# 28. DO NOT DESTROY USER DATA

Never delete the source PDF.

Never overwrite the original PDF.

Never recursively delete directories outside:

`<INSTALL_ROOT>`

without explicit permission.

Be particularly careful with commands such as:

```text
Remove-Item
rmdir
del
taskkill
```

Validate paths before destructive operations.

---

# 29. SECURITY

Do not expose a web server to the LAN or Internet unless explicitly required.

The application should preferably operate entirely as a local batch/CLI application.

If a local service is technically necessary:

* bind to localhost by default
* use an application-specific port
* document it
* shut it down when processing ends

Do not expose OCR services publicly.

---

# 30. DO NOT CONFUSE OCR WITH THE LLM

This system's primary task is:

```text
PDF → OCR/document parsing → Markdown
```

It is NOT primarily an LLM application.

Do not install a large local LLM merely for the sake of this pipeline unless research demonstrates that an LLM is genuinely required for document parsing.

The resulting Markdown will later be consumed by a separate local LLM/RAG system.

If an optional local LLM can substantially improve document structure or OCR correction, investigate it as an OPTIONAL second-stage feature, not as a mandatory dependency.

---

# 31. OPTIONAL SECOND-STAGE CLEANING

Investigate whether a lightweight local LLM can perform post-processing such as:

* OCR typo correction
* paragraph cleanup
* heading normalization
* removal of obvious repeated headers
* table cleanup
* Markdown normalization

But this must be optional.

Never let an LLM freely "rewrite" educational content.

The source OCR text must remain recoverable.

If implementing this feature, use:

```text
raw OCR
    ↓
structured Markdown
    ↓
optional LLM cleanup
    ↓
final Markdown
```

Keep the raw/initial output available.

---

# 32. OUTPUT DIRECTORY STRUCTURE

Prefer something like:

```text
output\
    BookName\
        BookName.md
        assets\
        metadata.json
        processing_report.txt
```

or another well-designed structure.

The output must be easy to move to another computer or feed into an LLM/RAG system.

---

# 33. METADATA

Generate a small metadata file, e.g.:

```json
{
  "source_file": "...",
  "page_count": 350,
  "processed_pages": 350,
  "failed_pages": 0,
  "ocr_used": true,
  "language": ["bn", "en"],
  "configuration": "balanced",
  "processing_time_seconds": 1234
}
```

Do not put unnecessary sensitive information in metadata.

---

# 34. RECOVERY / RESUME

If technically practical, implement resumable processing.

If a 350-page book fails at page 280, I should not have to start everything from page 1.

At minimum, preserve intermediate state sufficiently to diagnose and recover.

If resume support is not reliable with the chosen framework, document that limitation instead of pretending it exists.

---

# 35. TEST THE ACTUAL SYSTEM

Do not stop after installation.

Actually execute:

```text
start.bat
```

using a test PDF.

Verify:

* input prompt
* output prompt
* configuration loading
* PDF detection
* OCR
* Markdown creation
* output location
* logs
* metadata
* cleanup
* process termination

Then execute:

```text
stop.bat
```

while the application is running and verify that it safely stops ONLY the processes belonging to this project.

---

# 36. TEST CONFIGURATION SWITCHING

Test:

```text
default/balanced
high-performance
low-resource
high-quality
```

Verify that changing configuration actually changes the behavior.

Do not merely create configuration files without testing them.

---

# 37. FINAL VALIDATION

Before declaring the project complete, verify all of the following:

[ ] Installation is inside `<INSTALL_ROOT>`

[ ] No unnecessary global installation was performed

[ ] Bangla OCR works

[ ] Text PDFs work

[ ] Scanned PDFs work

[ ] Large PDFs work

[ ] Markdown is generated

[ ] Tables are handled

[ ] Headings are handled

[ ] Reading order is handled

[ ] Images are handled appropriately

[ ] Logs are generated

[ ] Metadata is generated

[ ] Temporary files are cleaned

[ ] GPU resources are released

[ ] Worker processes terminate

[ ] `start.bat` works

[ ] `stop.bat` works

[ ] Stop script does not kill unrelated processes

[ ] Default configuration works

[ ] High-performance configuration works

[ ] Low-resource configuration works

[ ] High-quality configuration works if implemented

[ ] Benchmark completed

[ ] README completed

[ ] Version documentation completed

[ ] Known limitations documented

[ ] No PDF is uploaded externally

[ ] No paid API is required for normal operation

---

# 38. FINAL REPORT

When everything is complete, give me a concise but technically useful final report containing:

## Installation

* installation path
* major components
* exact versions

## Architecture

Show:

```text
PDF
 ↓
PDF detection
 ↓
Text extraction OR OCR
 ↓
Layout analysis
 ↓
Structure reconstruction
 ↓
Markdown
 ↓
Optional cleanup
 ↓
LLM/RAG-ready document
```

## Usage

Show exactly how I use:

```text
start.bat
```

and:

```text
stop.bat
```

## Configuration

Explain:

* High Performance
* Balanced
* Low Resource
* High Quality, if available

## Benchmark

Show:

* test PDF size/pages
* processing time
* pages/minute
* RAM
* VRAM
* CPU/GPU usage
* failures

## Quality

Explain what was actually tested.

Do NOT invent accuracy percentages.

## Known limitations

Be honest about:

* difficult scans
* unusual fonts
* tables
* mathematics
* diagrams
* handwriting
* OCR errors
* complex layouts

## Future improvements

List useful optional improvements without installing unnecessary things now.

---

# 39. MOST IMPORTANT OPERATING RULE

Work autonomously whenever safe.

Do not repeatedly ask me for confirmation for ordinary non-destructive steps.

However, STOP and ask me when:

* administrator permission is required
* a manual download is unavoidable
* a license requires human acceptance
* a file must be manually placed somewhere
* a reboot is required
* a security setting must change
* a destructive action is necessary
* there is ambiguity that could cause data loss
* the selected technology requires an external/paid service
* you cannot reliably determine what a command will modify

When asking me for an action, give me:

1. What I need to do
2. Why it is necessary
3. Exact download/action required
4. Where to place the file
5. What result/path I should give back to you

Then WAIT.

Once I provide the requested result, continue the setup from where you stopped.

---

# 40. IMPORTANT: RESEARCH BEFORE FINAL ARCHITECTURE

You have permission to change the proposed architecture.

If your research determines that:

```text
Marker + Surya
```

is not the best solution for current Bangla NCTB PDFs, replace it with a better open-source architecture.

Likewise, investigate whether:

```text
PaddleOCR-VL
```

or another current OCR/document parser provides better results.

The final system should be selected based on:

**Bangla OCR quality + NCTB document structure + large PDF reliability + local operation + low cost + Windows compatibility + maintainability.**

Do not optimize for popularity.

---

# START

First:

1. Inspect the PC.
2. Identify the hardware/software environment.
3. Research current candidate technologies.
4. Compare them specifically for Bangla NCTB textbooks.
5. Select the architecture.
6. Explain the selected architecture briefly.
7. Then begin installation.

Do not make assumptions about missing dependencies.

Do not silently skip tests.

Do not declare success until the actual pipeline has been executed and validated.

The final objective is a polished, reusable local tool where I can simply:

**double-click `start.bat` → provide PDF path → optionally provide destination → wait → receive clean Markdown → all processing resources automatically shut down.**
