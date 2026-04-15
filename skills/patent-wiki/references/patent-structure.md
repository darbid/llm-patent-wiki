# Patent Document Structure

Understanding how patent documents are organized helps extract information accurately during Pass A decomposition.

---

## Standard Patent Sections

### 1. Header / Metadata

Located at the top of the document. Contains:
- **Patent number**: e.g., US20260072718A1
- **Inventors**: list of named inventors
- **Assignee**: company or entity that owns the patent
- **Filing date**: when the application was filed
- **Publication date**: when the application was published
- **Application number**: the serial number
- **Classifications**: IPC/CPC codes indicating the technical field

### 2. Title

A brief description of the invention. Often very generic.

### 3. Abstract

A concise summary (typically one paragraph) of the entire disclosure. Useful for initial orientation but not sufficient for detailed analysis.

### 4. Classifications

IPC (International Patent Classification) and CPC (Cooperative Patent Classification) codes. These indicate the technical domain.

### 5. Claims

The legal scope of the patent. This is the most critical section.

**Claim types**:
- **Independent claims**: Stand alone. Define the broadest scope. Start with preamble ("A method comprising...", "A system comprising...", "A non-transitory computer-readable medium...").
- **Dependent claims**: Reference a parent claim ("The method of claim 1, wherein..."). Add narrowing limitations.

**Claim language signals**:
- "comprising" = open-ended (allows additional elements)
- "consisting of" = closed (only listed elements)
- "may", "can", "in some embodiments" = optional features
- "wherein" = additional limitation on a previously introduced element
- "a", "an" = introduces a new element
- "the", "said" = refers back to a previously introduced element

### 6. Description

The detailed technical disclosure. Organized into subsections:

#### Cross-Reference to Related Applications
References to priority/parent applications.

#### Field of Disclosure
One sentence identifying the technical field.

#### Background
Describes the **problem** the invention solves. This is where prior art limitations are discussed. Key for extracting technical problems and prior art deltas.

#### Summary
High-level overview of the invention. Often mirrors the independent claims in prose form. Key for extracting the broad inventive concept.

#### Brief Description of Drawings
One-line descriptions of each figure. Key for building the figure index.

#### Detailed Description
The bulk of the patent. Describes:
- System architecture (with figure references)
- Component functions (with reference numbers like "engine 101")
- Process steps (with flow diagram references)
- Alternative embodiments
- Specific examples
- Parameters and ranges

Paragraphs are numbered: [0001], [0002], etc. These are your citation anchors.

---

## Extraction Tips

### For Claims
- Read every claim completely
- Map dependency chains (claim 2 depends on claim 1, claim 3 depends on claim 1, etc.)
- Decompose each independent claim into discrete elements
- Identify whether each element is required ("comprising") or optional ("may", "in some embodiments")

### For Figures
- Cross-reference the "Brief Description of Drawings" with the Detailed Description
- Extract all reference numbers (e.g., "101", "102a") and their names
- Note the figure type (block diagram, flow chart, etc.)

### For Technical Terms
- Look for explicit definitions: "As used herein, [term] refers to..."
- Look for implicit definitions: first use in context with explanation
- Note where terms are used in claims vs. description only

### For Embodiments
- Look for markers: "In one embodiment...", "In another embodiment...", "In some implementations..."
- Distinguish required from optional features
- Note which figures illustrate which embodiments

### For Parameters
- Look for specific values, ranges, or thresholds
- Note whether they are claim-level or description-level
- Distinguish "preferably" ranges from absolute limits

### For Prior Art
- Focus on the Background section
- Look for "However," or "Unfortunately," transitions that identify limitations
- Cross-reference any cited patent numbers or publications
