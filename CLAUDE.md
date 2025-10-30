# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a single-page web application that converts images to PDF format. The entire application is self-contained in a single HTML file ([index.html](index.html)) with no build process or dependencies to install.

## Architecture

### Technology Stack
- **React 18**: Loaded via CDN (unpkg.com)
- **Babel Standalone**: For JSX transpilation in the browser
- **Tailwind CSS**: Via CDN for styling
- **jsPDF**: For PDF generation (cdnjs)
- All code runs client-side in the browser

### Application Structure
The app is a single React component (`ImageToPdfConverter`) embedded in [index.html](index.html) using inline `<script type="text/babel">` tags:

1. **State Management** (lines 52-54):
   - `images`: Array of uploaded image objects with id, file, preview URL, and name
   - `isDragging`: Boolean for drag-and-drop UI feedback
   - `isProcessing`: Boolean to show PDF generation progress

2. **Core Functions**:
   - `addImages()` (line 90): Creates preview URLs and adds images to state
   - `removeImage()` (line 101): Removes image and cleans up preview URL
   - `moveImage()` (line 109): Reorders images in the array
   - `generatePDF()` (line 123): Main PDF generation logic

3. **PDF Layout** (lines 137-149):
   - A4 portrait format (210mm × 297mm)
   - 4 images per page in 2×2 grid layout
   - Images are aspect-ratio-fitted within their grid cells
   - Auto-pagination when more than 4 images

### Key Implementation Details

**Image Processing Flow**:
1. Images uploaded via drag-drop or file input
2. Preview URLs created with `URL.createObjectURL()`
3. Images stored in state array with unique IDs
4. Preview URLs must be revoked to prevent memory leaks

**PDF Generation** (line 123):
- Creates Image elements and waits for load
- Calculates aspect ratio to fit images in grid cells
- Uses jsPDF's `addImage()` to place images at calculated positions
- Adds new pages automatically every 4 images

## Development

### Running the Application
Simply open [index.html](index.html) in a web browser:
```bash
# Option 1: Direct file opening
open index.html  # macOS
xdg-open index.html  # Linux

# Option 2: Local server (recommended for testing)
python3 -m http.server 8000
# Then visit http://localhost:8000
```

### Making Changes
Since this is a single-file application with no build step:
1. Edit [index.html](index.html) directly
2. Refresh browser to see changes
3. Use browser DevTools for debugging

### Testing Considerations
- Test with various image formats (JPG, PNG, GIF, WebP)
- Test with different image sizes and aspect ratios
- Test pagination with more than 4 images
- Verify memory cleanup (check for URL.revokeObjectURL calls)
- Test drag-and-drop functionality across different browsers

## Code Modification Notes

When modifying the PDF layout:
- Page dimensions are in millimeters (A4 standard)
- Grid positions calculated in `positions` array (line 144)
- Margin and spacing constants defined at lines 139-142
- Aspect ratio preservation logic at lines 167-180

When adding features:
- All React hooks and components must be defined within the `<script type="text/babel">` tag
- External dependencies must be loaded via CDN in the `<head>` section
- State changes trigger re-renders automatically via React
