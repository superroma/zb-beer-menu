# Figma Beer Card Template Creation Guide

This guide will help you create the beer card templates in Figma for the zb-beer-menu project.

## Template Specifications

### Card Dimensions

- **Size**: 85mm x 55mm (standard business card size for easy printing)
- **Or**: 90mm x 50mm (alternative credit card size)
- **Bleed**: Add 3mm bleed on all sides if printing service requires it

## Step-by-Step Instructions

### 1. Create Front Side Template

1. **Create Frame**

   - Press `F` or select Frame tool
   - Name it: `BeerCard_Front_Template`
   - Set dimensions: 85mm x 55mm (or 255px x 165px at 72 DPI)
   - Position: 0, 0

2. **Add Background Rectangle**

   - Press `R` for rectangle tool
   - Draw rectangle covering entire frame
   - Name it: `bg_front`
   - Fill: #F5DEB3 (wheat color - placeholder)
   - Make sure it's the bottom layer

3. **Add Beer Logo Placeholder**

   - Press `R` for rectangle tool
   - Create rectangle: 120px x 120px
   - Position: Center horizontally, Y: 20px from top
   - Name it: `logo_image`
   - Fill: #CCCCCC (gray placeholder)
   - Optional: Add text "LOGO" centered inside

4. **Add Country Flag Placeholder**

   - Press `R` for rectangle tool
   - Create rectangle: 40px x 30px
   - Position: Top-right corner with 10px margin
   - Name it: `flag_image`
   - Fill: #DDDDDD (light gray placeholder)
   - Optional: Add text "FLAG" centered inside

5. **Add Beer Name Text**
   - Press `T` for text tool
   - Click below the logo placeholder
   - Name it: `beer_name`
   - Text: "Beer Name Here"
   - Font: Bold, 18-24pt (e.g., Roboto Bold, Inter Bold)
   - Alignment: Center
   - Color: #000000 (black)
   - Position: Center horizontally, bottom area of card

### 2. Create Back Side Template

1. **Create Frame**

   - Press `F` or select Frame tool
   - Name it: `BeerCard_Back_Template`
   - Set dimensions: 85mm x 55mm (same as front)
   - Position: 300px to the right of front template (for easy viewing)

2. **Add Background Rectangle**

   - Press `R` for rectangle tool
   - Draw rectangle covering entire frame
   - Name it: `bg_back`
   - Fill: #F5DEB3 (wheat color - placeholder)
   - Make sure it's the bottom layer

3. **Add Text Fields (Left Column)**

   Starting from top-left with 10px margin:

   **Country:**

   - Press `T` for text tool
   - Name: `country`
   - Text: "Country: Czech Republic"
   - Font: Regular, 10-12pt
   - Position: X: 10px, Y: 10px

   **Style:**

   - Name: `style`
   - Text: "Style: IPA"
   - Font: Regular, 10-12pt
   - Position: X: 10px, Y: 30px

   **ABV:**

   - Name: `abv`
   - Text: "ABV: 6.5%"
   - Font: Regular, 10-12pt
   - Position: X: 10px, Y: 50px

4. **Add Text Fields (Right Column)**

   **EBC:**

   - Name: `ebc`
   - Text: "EBC: 12"
   - Font: Regular, 10-12pt
   - Position: X: 135px, Y: 10px

   **IBU:**

   - Name: `ibu`
   - Text: "IBU: 45"
   - Font: Regular, 10-12pt
   - Position: X: 135px, Y: 30px

   **Brewery:**

   - Name: `brewery`
   - Text: "Brewery: Example Brewery"
   - Font: Regular, 10-12pt
   - Position: X: 10px, Y: 70px

5. **Add Facts Text Box**
   - Press `T` for text tool
   - Create text box spanning most of the bottom area
   - Name: `facts`
   - Text: "Interesting facts about this beer will appear here. This section will contain 50-100 words of information about the beer's history, brewing process, or unique characteristics."
   - Font: Regular, 9-10pt
   - Width: Card width - 20px (margins)
   - Position: X: 10px, Y: 95px
   - Enable text wrapping

### 3. Optional: Add Auto-Layout

For easier maintenance:

1. Select `BeerCard_Front_Template` frame

   - Consider using auto-layout (Shift + A) for text elements
   - This helps maintain consistent spacing

2. Select `BeerCard_Back_Template` frame
   - Consider organizing text fields in auto-layout containers
   - Group related fields (country/style/abv, ebc/ibu)

### 4. Layer Organization

Make sure layers are properly organized in each frame:

**Front Template layers (top to bottom):**

1. beer_name
2. flag_image
3. logo_image
4. bg_front

**Back Template layers (top to bottom):**

1. facts
2. brewery
3. ibu
4. ebc
5. abv
6. style
7. country
8. bg_back

### 5. Testing the Template

Before using with Python scripts:

1. **Duplicate the templates** to test
2. **Try replacing text** in duplicated versions
3. **Test with different text lengths** to ensure it fits
4. **Verify all layers are named correctly** (case-sensitive!)

### 6. Get File Information for .env

After creating the template:

1. **Get File Key**:

   - From your URL: `https://www.figma.com/design/6B5cyazXczgtiV3TJtorwK/...`
   - File key: `6B5cyazXczgtiV3TJtorwK`

2. **Get Node IDs**:

   - Right-click on `BeerCard_Front_Template` frame
   - Select "Copy/Paste as" → "Copy link to selection"
   - Extract node-id from URL (e.g., `123:456`)
   - Repeat for `BeerCard_Back_Template`

3. **Add to .env file**:
   ```
   FIGMA_FILE_KEY=6B5cyazXczgtiV3TJtorwK
   FIGMA_FRONT_TEMPLATE_NODE_ID=<your-front-node-id>
   FIGMA_BACK_TEMPLATE_NODE_ID=<your-back-node-id>
   ```

## Design Tips

### Colors

- Use neutral placeholder colors that will be replaced by EBC-derived colors
- Ensure text has good contrast against backgrounds
- Consider using white or black text depending on beer color

### Typography

- Use fonts available in both Figma and exportable to SVG
- Avoid very thin fonts that might not print well
- Ensure Cyrillic character support for Russian text

### Images

- Keep image placeholders at reasonable sizes
- Beer logo: Square format (120x120px)
- Country flag: Standard flag ratio (4:3 or 3:2)

### Print Considerations

- Use RGB color mode (will convert for print later)
- Keep text size readable (minimum 8-9pt)
- Ensure 3mm margin from edges for important content
- Add bleed if required by printing service

## Next Steps

After creating the template:

1. Test with one beer manually to verify layout
2. Note the exact node IDs of both templates
3. Update your .env file with the file key and node IDs
4. Run the Python scripts to generate actual beer cards

## Troubleshooting

**Layer names not matching?**

- Ensure exact spelling (case-sensitive)
- Check for extra spaces in layer names
- Use underscore naming convention as specified

**Text not fitting?**

- Increase frame size or decrease font size
- Adjust text box dimensions
- Consider using auto-layout for dynamic sizing

**Images not appearing?**

- Verify rectangle layers are named correctly
- Check that fills are enabled
- Ensure layers are not locked or hidden
