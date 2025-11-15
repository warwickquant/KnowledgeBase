# Copilot Instructions for Warwick Quant Knowledge Base

## Repository Purpose
This is a modular knowledge base for a student-led quantitative finance society. Content is organized into distinct sections covering active research projects, learning resources, textbooks, tutorials, career preparation, competitions, code examples, and community contributions.

## Architecture Overview

### Modular Structure
Content is **distributed across dedicated directories** rather than consolidated in a single README:
- `docs/projects/` - Active research with detailed project files
- `docs/resources/` - Online resources (websites, social media, podcasts, YouTube)
- `docs/textbooks/` - Textbook recommendations with descriptions
- `docs/tutorials/` - How-to guides (research pipeline, backtesting)
- `docs/careers/` - Interview preparation resources
- `docs/competitions/` - Trading competitions and hackathons
- `docs/community/` - Contribution guidelines and paper summaries
- `examples/` - Code templates and implementations

### README as Navigation Hub
The main `README.md` serves as a **landing page with quick navigation links** to all sections. It does NOT contain the full content - just summaries and links to dedicated files.

## Content Management Conventions

### Projects (`docs/projects/`)
Each project has **its own markdown file** with standardized sections:
- Overview, Research Questions, Methodology, Data Requirements
- Literature Review, Current Status, Next Steps, Resources
- Template exists in crypto-options.md, ai-stat-arb.md, prediction-markets.md

**When adding projects**: Create new `.md` file following existing template, update `docs/projects/README.md`

### Resources (`docs/resources/`)
**Four separate files** for different resource types:
- `websites.md` - Include brief description of each website's value
- `socials.md` - X/Twitter accounts with expertise area noted
- `podcasts.md` - Include host, focus, and "why listen"
- `youtube.md` - Categorize by trading firms vs educational content

**Link format**: Use descriptive markdown links, not bare URLs (unlike original README)

### Textbooks (`docs/textbooks/`)
**Three discipline files** (mathematics.md, statistics.md, finance.md):
- Each book entry includes **description explaining what it covers and why it's valuable**
- Full citations with author, year, title, publisher, link
- **No reading roadmaps or learning pathways** (removed per user request)
- Note incomplete entries (e.g., "Probability with Martingales" missing full citation)

**Format**: Heading per book title, followed by author/publisher block, then description paragraph

### Tutorials (`docs/tutorials/`)
**Only two tutorials** (per user request):
- `research-pipeline.md` - Comprehensive 11-step guide from idea to execution
- `backtesting-framework.md` - Technical guide to building backtest systems

Both are **detailed, practical guides with code examples**

### Careers (`docs/careers/`)
**Single file** `interview-prep.md` covering:
- Programming practice (LeetCode guidance)
- Quant finance problems (OpenQuant)
- Books (Cracking the Coding Interview, etc.)
- Interview types, firm-specific prep, timelines
- **No companies.md or internship-guide.md** (removed per user request)

### Competitions & Examples
- `docs/competitions/README.md` - Single file listing all competitions
- `examples/README.md` - Overview with quick code snippets, structure for future expansion

### Community (`docs/community/`)
- `README.md` - Contribution guidelines, quality standards, code of conduct
- `papers.md` - Research paper summaries (currently placeholder, template provided)
- **Papers section moved here** (not separate top-level section per user request)

## File Organization Principles

1. **Modularity**: Each section is self-contained, can be updated independently
2. **Discoverability**: Clear navigation from README to all content
3. **Scalability**: Easy to add new projects, resources, or sections
4. **Consistency**: Similar content follows same structure/format
5. **Cross-linking**: Files link to related sections (e.g., projects link to tutorials)

## Updating Content

### Adding Resources
1. Add to appropriate file in `docs/resources/`
2. Include description/context, not just bare URL
3. Maintain alphabetical or categorical ordering

### Adding Textbooks
1. Add to correct discipline file in `docs/textbooks/`
2. Include full citation + description paragraph
3. Explain **what the book covers** and **why it's valuable for quant finance**

### Adding Projects
1. Create new `.md` file in `docs/projects/` using existing template
2. Update `docs/projects/README.md` to reference new project
3. Update main README.md if project is featured

### Adding Code Examples
1. Add to `examples/` directory with clear documentation
2. Update `examples/README.md` if adding new category
3. Follow best practices: comments, error handling, test cases

## Target Audience
University students studying quantitative finance, with emphasis on:
- Active research (projects section provides framework)
- Practical implementation (tutorials and examples)
- Career preparation (interview resources, competitions)
- Continuous learning (textbooks, online resources)

## Key Differences from Original
- **Original**: Single README with all content
- **Current**: Modular structure with navigation README
- **Original**: Bare URL lists
- **Current**: Descriptive entries with context
- **Original**: Incomplete textbook citations
- **Current**: Full citations + descriptions (though some still need completion)
- **New additions**: Tutorials, competitions, examples, community sections

## Maintenance Notes
- This is primarily a **documentation repository**, not code
- Changes involve updating markdown files, adding resources, expanding sections
- No build process or deployment pipeline
- Version control via Git, contributions via pull requests
- `.github/copilot-instructions.md` should be updated when architecture changes

