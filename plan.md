# Beer Menu App Plan

## Goal

Create beer menu source to pass to the printing service.

Requirement of the service - SVG file with text converted to paths.

Use Figma for design and export to SVG.

Use untappd for business API to get beer data. https://docs.business.untappd.com/#list-all-items

Use OpenAI API to translate texts to Russian and generate interesting facts.

Basic layout:
Front side - beer logo image, beer country flag image, beer name.
Back side - country, style, abv, color, brewery, maybe some other beer attributes like EBC or IBU and some 50-100 words of interesting facts about the beer.
Background of the page roughly matching the beer color (lets use EBC to RGB conversion).

## Technical Stack

Python 3.12 with .venv virtual environment and pip for package management

scripts:

- fetchBeerData.py - fetch beer data from Untappd API and save to JSON file.
- processBeerData.py - read beer data from JSON file, translate texts to Russian using OpenAI API, generate interesting facts using OpenAI API, update colors, pick necessary images, and save to another JSON file.
- updateFigma.py - read processed beer data from JSON file and update Figma design using Figma API and the template.
- exportSVG.py - export SVG files from Figma using Figma API.

- store data in data/, images in images/ and result svgs in svgs/
- configuration is from env variables or .env file

Beer logo image.
Use images/logos/{beer_slug}.png if exists, otherwise in fetchBeerData.ts download from Untappd API and save to images/logos/{beer_slug}.png

Country flag image.
Use images/flags/{country_code}.png if exists, otherwise in processBeerData.ts download from a free API and save to images/flags/{country_code}.png

Figma template - give instructions how to create it in plan.md

Error handling - make it simple for now, just log errors to console, no validation, no retries.

---

## Implementation Action Plan

### Phase 1: Project Setup

#### 1.1 Initialize Project Structure

- [ ] Create Python 3.12 virtual environment: `python3.12 -m venv .venv`
- [ ] Activate virtual environment: `source .venv/bin/activate` (macOS/Linux) or `.venv\Scripts\activate` (Windows)
- [ ] Create directory structure:
  ```
  /data
  /images
    /logos
    /flags
  /svgs
  /src
  ```
- [ ] Create `requirements.txt` for pip dependencies
- [ ] Create `.env.example` file with required environment variables
- [ ] Create `.gitignore` for Python, .venv, data files, and API keys

#### 1.2 Install Dependencies

Install packages using pip in the activated .venv:

```bash
pip install requests python-dotenv openai pillow
```

Or create `requirements.txt` with:

```
requests>=2.31.0
python-dotenv>=1.0.0
openai>=1.0.0
Pillow>=10.0.0
```

Then install with: `pip install -r requirements.txt`

#### 1.3 Environment Configuration

Create `.env` file with:

- [ ] `UNTAPPD_API_KEY` - Untappd Business API key
- [ ] `UNTAPPD_MENU_ID` - Your Untappd menu ID
- [ ] `OPENAI_API_KEY` - OpenAI API key
- [ ] `FIGMA_ACCESS_TOKEN` - Figma personal access token
- [ ] `FIGMA_FILE_KEY` - Figma file ID for the template

### Phase 2: Figma Template Setup

#### 2.1 Create Figma Template Instructions

The template should contain:

**Front Side Frame:**

- [ ] Frame name: `BeerCard_Front_Template`
- [ ] Rectangle for background (named `bg_front`)
- [ ] Image placeholder for beer logo (named `logo_image`)
- [ ] Image placeholder for country flag (named `flag_image`)
- [ ] Text layer for beer name (named `beer_name`)

**Back Side Frame:**

- [ ] Frame name: `BeerCard_Back_Template`
- [ ] Rectangle for background (named `bg_back`)
- [ ] Text layer for country (named `country`)
- [ ] Text layer for style (named `style`)
- [ ] Text layer for ABV (named `abv`)
- [ ] Text layer for EBC (named `ebc`)
- [ ] Text layer for IBU (named `ibu`)
- [ ] Text layer for brewery (named `brewery`)
- [ ] Text box for interesting facts (named `facts`)

**Layout:**

- [ ] Set appropriate dimensions for print
- [ ] Use auto-layout where appropriate
- [ ] Ensure text layers have sufficient size and proper fonts

### Phase 3: Core Script Development

#### 3.1 fetchBeerData.py

- [ ] Create function to authenticate with Untappd API
- [ ] Implement `fetch_menu_items()` to get all beers from menu
- [ ] For each beer, extract:
  - beer_id, beer_slug, beer_name
  - brewery_name
  - style
  - abv, ibu, ebc (if available)
  - country, country_code
  - label_image_url (for logo)
- [ ] Download beer logos to `images/logos/{beer_slug}.png`
- [ ] Save raw data to `data/raw_beer_data.json`
- [ ] Add error logging for failed API calls

#### 3.2 processBeerData.py

- [ ] Load raw beer data from `data/raw_beer_data.json`
- [ ] For each beer:
  - [ ] Translate beer name to Russian using OpenAI API
  - [ ] Generate 50-100 word interesting facts using OpenAI API (in Russian)
  - [ ] Convert EBC value to RGB color using EBC-to-RGB conversion formula
  - [ ] Download country flag if not exists in `images/flags/{country_code}.png`
    - Use API like `https://flagcdn.com/{country_code}.png`
- [ ] Save processed data to `data/processed_beer_data.json` with structure:
  ```json
  {
    "beer_id": "...",
    "beer_slug": "...",
    "original_name": "...",
    "russian_name": "...",
    "brewery": "...",
    "country": "...",
    "country_code": "...",
    "style": "...",
    "abv": "...",
    "ibu": "...",
    "ebc": "...",
    "background_color": { "r": 0, "g": 0, "b": 0 },
    "interesting_facts_ru": "...",
    "logo_path": "images/logos/...",
    "flag_path": "images/flags/..."
  }
  ```

#### 3.3 updateFigma.py

- [ ] Load processed beer data from `data/processed_beer_data.json`
- [ ] Connect to Figma API using access token
- [ ] For each beer:
  - [ ] Duplicate template frames (front & back)
  - [ ] Rename frames to `{beer_slug}_front` and `{beer_slug}_back`
  - [ ] Update front side:
    - Set background color
    - Upload and set logo image
    - Upload and set flag image
    - Update beer name text
  - [ ] Update back side:
    - Set background color
    - Update all text fields (country, style, abv, ebc, ibu, brewery, facts)
- [ ] Save Figma file

#### 3.4 exportSVG.py

- [ ] Connect to Figma API
- [ ] Get all beer card frames (filter by naming pattern)
- [ ] For each frame (front & back):
  - [ ] Export as SVG with text converted to outlines/paths
  - [ ] Save to `svgs/{beer_slug}_front.svg` and `svgs/{beer_slug}_back.svg`
- [ ] Log export completion for each file

### Phase 4: Utility Functions

#### 4.1 Common Utilities (utils.py)

- [ ] `load_env()` - load environment variables
- [ ] `load_json(filepath)` - load JSON file
- [ ] `save_json(data, filepath)` - save JSON file
- [ ] `ebc_to_rgb(ebc_value)` - convert EBC to RGB color
- [ ] `download_image(url, filepath)` - download and save image
- [ ] `log_error(message)` - simple error logging

#### 4.2 EBC to RGB Conversion

Implement formula or lookup table:

- EBC 4-8 (Pale): Light yellow/gold
- EBC 8-20 (Amber): Amber/orange
- EBC 20-40 (Brown): Brown
- EBC 40+ (Black): Dark brown/black

### Phase 5: Testing & Execution

#### 5.1 Testing Individual Scripts

- [ ] Test `fetchBeerData.py` with sample menu
- [ ] Verify JSON output structure
- [ ] Test `processBeerData.py` with sample data
- [ ] Verify OpenAI API responses
- [ ] Test `updateFigma.py` with one beer
- [ ] Test `exportSVG.py` export quality

#### 5.2 Create Main Orchestrator (optional)

- [ ] Create `main.py` to run all scripts in sequence
- [ ] Add command-line arguments for individual script execution

#### 5.3 Documentation

- [ ] Update README.md with:
  - Setup instructions
  - API key configuration
  - Usage instructions
  - Troubleshooting tips

### Phase 6: Enhancements (Future)

- [ ] Add validation for beer data completeness
- [ ] Add retry logic for API failures
- [ ] Add caching for OpenAI translations
- [ ] Add command-line interface with arguments
- [ ] Add batch processing options
- [ ] Add preview generation (PNG/PDF)
- [ ] Add tests for utility functions

---

## Development Order

1. **Start with Phase 1** - Set up project structure and dependencies
2. **Phase 2** - Create Figma template manually
3. **Phase 4.1** - Build utility functions first (they'll be used by all scripts)
4. **Phase 3.1** - Implement fetchBeerData.py
5. **Phase 3.2** - Implement processBeerData.py
6. **Phase 3.3** - Implement updateFigma.py
7. **Phase 3.4** - Implement exportSVG.py
8. **Phase 5** - Test and refine

---

## Notes

- Keep scripts modular and independent where possible
- Use descriptive variable names and add comments
- Log progress at each major step
- Test with 1-2 beers before processing entire menu
- Back up Figma file before bulk updates
