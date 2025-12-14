# Product Requirements Document: ReFi Orbifier

## 1. Overview

### 1.1 Product Name
ReFi Orbifier

### 1.2 Product Summary
A single-page web application that allows users to upload a profile photo, automatically removes the background, and replaces it with the signature ReFi DAO gradient orb. Users can download their "orbified" profile picture in X-friendly formats and share it directly to social media.

### 1.3 Problem Statement
Members of the ReFi (Regenerative Finance) community want to visually identify themselves as part of the movement on social media. Currently, creating an orbified profile picture requires design skills and software like Photoshop. This tool democratizes access to the ReFi visual identity.

### 1.4 Target Users
- ReFi DAO community members
- Regenerative finance enthusiasts
- Web3/blockchain community members interested in ReFi
- Anyone wanting to show support for the regenerative finance movement

### 1.5 Success Metrics
- Number of profile pictures generated
- Social shares with #ReFi hashtag
- User completion rate (upload → download)

---

## 2. Technical Requirements

### 2.1 Tech Stack
- **Frontend:** Vanilla HTML5, CSS3, JavaScript (ES6+)
- **Architecture:** Single-page application, no server required
- **Hosting:** Static file hosting (GitHub Pages, Netlify, Vercel, etc.)
- **External API:** remove.bg API for background removal

### 2.2 Browser Support
- Chrome 90+
- Firefox 88+
- Safari 14+
- Edge 90+

### 2.3 File Structure
```
/refi-orbifier
├── index.html              # Main application file
├── PRD.md                  # This document
├── tasks/                  # Task management (task-master)
│   └── tasks.json
└── assets/
    ├── favicon.ico         # ReFi-branded favicon
    └── og-image.png        # Open Graph image for social sharing
```

---

## 3. Design Specifications

### 3.1 Brand Colors
| Name | Hex Code | Usage |
|------|----------|-------|
| ReFi Space | #172027 | Primary background |
| ReFi Blue | #4571E1 | Orb gradient, accents |
| ReFi Green | #71E3BA | Orb gradient |
| ReFi Yellow | #FFFA7E | Orb gradient |
| ReFi Pink | #DE9AE9 | Orb gradient |
| ReFi Cloud | #F1F0FF | Light text, borders |

### 3.2 Typography
- **Font Family:** Switzer (from Fontshare)
- **Weights:** 300 (Light), 500 (Medium)
- **Fallback:** -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif

### 3.3 The ReFi Orb
The signature ReFi orb is a circular gradient with the following characteristics:
- **Type:** Conic/angular gradient
- **Shape:** Perfect circle
- **Colors:** Smooth transition through Blue → Cyan → Green → Yellow → Pink → Purple → Blue
- **Texture:** Subtle noise/grain overlay (5-10% opacity)
- **Placement:** Centered behind the subject, sized to frame head and shoulders

### 3.4 Visual Style
- Dark mode aesthetic matching ReFi DAO branding
- Subtle radial gradients in background
- Minimal, clean interface
- Smooth transitions and hover states

---

## 4. Functional Requirements

### 4.1 API Key Management
**Priority:** P0 (Required)

**Description:** Users must provide their own remove.bg API key to use the application.

**Requirements:**
- [ ] Input field for remove.bg API key
- [ ] "Get API Key" link to remove.bg signup (https://www.remove.bg/api)
- [ ] Store API key in browser localStorage
- [ ] Show/hide toggle for API key visibility
- [ ] Validate API key format before storing
- [ ] Clear API key option
- [ ] Display remaining API credits if available

**Acceptance Criteria:**
- User can enter and save their API key
- API key persists across browser sessions
- Invalid keys show appropriate error message

---

### 4.2 Image Upload
**Priority:** P0 (Required)

**Description:** Users can upload their profile photo through multiple methods.

**Requirements:**
- [ ] Drag-and-drop zone with visual feedback
- [ ] Click to browse file picker
- [ ] Support for PNG, JPG, JPEG, WEBP formats
- [ ] Client-side file validation (type and size)
- [ ] Maximum file size: 10MB
- [ ] Preview of uploaded image before processing
- [ ] Clear/remove uploaded image option

**Acceptance Criteria:**
- Dragging a file shows visual drop zone highlight
- Invalid files show descriptive error messages
- Large files are rejected with size guidance

---

### 4.3 Background Removal
**Priority:** P0 (Required)

**Description:** Remove the background from the uploaded photo using remove.bg API.

**Requirements:**
- [ ] Send image to remove.bg API endpoint
- [ ] Handle API authentication with user's key
- [ ] Show loading state during processing
- [ ] Display progress indicator
- [ ] Handle API errors gracefully
- [ ] Support retry on failure
- [ ] Cache result to avoid re-processing

**API Details:**
- Endpoint: `POST https://api.remove.bg/v1.0/removebg`
- Headers: `X-Api-Key: {user_api_key}`
- Body: FormData with `image_file` field
- Response: PNG with transparent background

**Error Handling:**
- 402: Insufficient credits → Show upgrade message
- 400: Invalid image → Show format guidance
- 401: Invalid API key → Prompt to re-enter key
- 429: Rate limited → Show retry message

**Acceptance Criteria:**
- Background is cleanly removed from portrait photos
- Processing time is clearly indicated
- All error states have user-friendly messages

---

### 4.4 Orb Compositing
**Priority:** P0 (Required)

**Description:** Composite the background-removed image with the ReFi orb gradient.

**Requirements:**
- [ ] Create HTML Canvas at 400x400 pixels
- [ ] Fill background with ReFi Space (#172027)
- [ ] Draw circular orb gradient (conic gradient)
- [ ] Apply noise texture overlay
- [ ] Scale and center foreground image
- [ ] Maintain aspect ratio of original photo
- [ ] Position subject appropriately (head near top third)

**Canvas Compositing Layers (bottom to top):**
1. Solid background (#172027)
2. Orb gradient circle (centered, ~320px diameter)
3. Noise texture (5% opacity, multiply blend)
4. Foreground image (subject with transparent background)

**Acceptance Criteria:**
- Output matches visual style of reference images
- Subject is well-framed within the orb
- Gradient transitions are smooth
- Noise texture is subtle but visible

---

### 4.5 Image Preview
**Priority:** P0 (Required)

**Description:** Show the final orbified result before download.

**Requirements:**
- [ ] Display preview at reasonable size (300-400px)
- [ ] Show loading skeleton during processing
- [ ] Smooth transition when image is ready
- [ ] Option to zoom/enlarge preview

**Acceptance Criteria:**
- Preview accurately represents downloaded image
- Loading states are not jarring

---

### 4.6 Download Functionality
**Priority:** P0 (Required)

**Description:** Allow users to download their orbified profile picture.

**Requirements:**
- [ ] Download as PNG format
- [ ] Output size: 400x400 pixels (X profile picture size)
- [ ] Filename: `refi-orbified-pfp.png`
- [ ] Single-click download (no modal)
- [ ] Works on mobile devices

**Acceptance Criteria:**
- Downloaded image is high quality PNG
- File downloads immediately on click
- Works across all supported browsers

---

### 4.7 Share to X (Twitter)
**Priority:** P1 (Important)

**Description:** Pre-drafted tweet for sharing the orbified profile picture.

**Requirements:**
- [ ] "Share on X" button
- [ ] Open Twitter Web Intent in new tab
- [ ] Pre-filled tweet text with:
  - Announcement of new orbified PFP
  - Mention of @ReFiDAOist
  - Mention of @iamjohnellison
  - #ReFi hashtag
  - #RegenerativeFinance hashtag
- [ ] Instruction to attach downloaded image

**Tweet Template:**
```
Just orbified my PFP with the ReFi Orbifier! 🌈

Thanks @ReFiDAOist & @iamjohnellison for this awesome tool!

#ReFi #RegenerativeFinance

[Attach your downloaded image]
```

**Note:** Twitter Web Intent API does not support image attachments, so users must manually attach the downloaded image.

**Acceptance Criteria:**
- Share button opens Twitter with pre-filled text
- Tweet mentions correct accounts
- Clear instruction about attaching image

---

### 4.8 Responsive Design
**Priority:** P1 (Important)

**Description:** Application works well on all device sizes.

**Requirements:**
- [ ] Mobile-first design approach
- [ ] Breakpoints: 480px, 768px, 1024px
- [ ] Touch-friendly controls (min 44px tap targets)
- [ ] Stacked layout on mobile
- [ ] Side-by-side preview on larger screens

**Acceptance Criteria:**
- Fully usable on iPhone SE size and up
- No horizontal scrolling on any device
- All interactive elements are easily tappable

---

## 5. User Interface

### 5.1 Page Structure

```
┌─────────────────────────────────────────────────────┐
│                     HEADER                          │
│  [ReFi Orb Logo]  ReFi Orbifier                    │
│  Transform your PFP with the ReFi orb              │
├─────────────────────────────────────────────────────┤
│                  API KEY SECTION                    │
│  ┌─────────────────────────────────────────────┐   │
│  │  remove.bg API Key  [Get free key →]        │   │
│  │  [••••••••••••••••••••••••] [👁] [Save]     │   │
│  └─────────────────────────────────────────────┘   │
├─────────────────────────────────────────────────────┤
│                  UPLOAD SECTION                     │
│  ┌─────────────────────────────────────────────┐   │
│  │                                              │   │
│  │         📷 Drop your photo here             │   │
│  │            or click to browse                │   │
│  │                                              │   │
│  │         PNG, JPG up to 10MB                  │   │
│  └─────────────────────────────────────────────┘   │
├─────────────────────────────────────────────────────┤
│                  RESULT SECTION                     │
│              (shown after processing)               │
│                                                     │
│              ┌──────────────────┐                  │
│              │                  │                  │
│              │    [Orbified     │                  │
│              │     Preview]     │                  │
│              │                  │                  │
│              └──────────────────┘                  │
│                                                     │
│        [📥 Download PNG]  [🐦 Share on X]          │
├─────────────────────────────────────────────────────┤
│                     FOOTER                          │
│  Made with 💚 for the ReFi community               │
│  Built by @iamjohnellison                          │
└─────────────────────────────────────────────────────┘
```

### 5.2 States

**Empty State:**
- No API key: Show API key input prominently
- API key saved: Show upload zone

**Loading States:**
- Uploading: Show file name with progress bar
- Removing background: Show "Removing background..." with spinner
- Compositing: Show "Creating your orb..." with spinner

**Success State:**
- Show orbified preview
- Show download and share buttons
- Option to create another

**Error States:**
- Invalid file type: "Please upload a PNG, JPG, or WEBP image"
- File too large: "Image must be under 10MB"
- API key invalid: "Invalid API key. Please check and try again."
- API error: "Something went wrong. Please try again."
- No face detected: "We couldn't detect a person in this image. Try a different photo."

---

## 6. Non-Functional Requirements

### 6.1 Performance
- Initial page load: < 2 seconds
- Time to interactive: < 3 seconds
- Background removal: Dependent on remove.bg API (~3-5 seconds typical)
- Canvas compositing: < 500ms

### 6.2 Accessibility
- WCAG 2.1 AA compliance
- Keyboard navigation support
- Screen reader compatible
- Sufficient color contrast ratios
- Focus indicators on interactive elements

### 6.3 Security
- API keys stored only in localStorage (never sent to any server except remove.bg)
- No tracking or analytics (privacy-respecting)
- HTTPS required for production deployment
- Content Security Policy headers recommended

### 6.4 SEO & Social
- Descriptive page title
- Meta description
- Open Graph tags for social sharing
- Twitter Card meta tags

---

## 7. Future Considerations (Out of Scope)

These features are not part of the initial release but may be considered for future versions:

- Multiple orb color presets
- Custom orb colors
- Different output sizes (LinkedIn, Discord, etc.)
- Batch processing
- Gallery of community orbified PFPs
- Server-side processing to avoid API key requirement
- Integration with other background removal services

---

## 8. Dependencies

### 8.1 External Services
| Service | Purpose | Pricing |
|---------|---------|---------|
| remove.bg | Background removal API | Free: 50 images/month, Paid plans available |

### 8.2 External Resources
| Resource | Purpose | License |
|----------|---------|---------|
| Switzer Font | Typography | Free (Fontshare) |

---

## 9. Launch Checklist

- [ ] All P0 features implemented and tested
- [ ] Cross-browser testing complete
- [ ] Mobile testing complete
- [ ] Accessibility audit passed
- [ ] Performance benchmarks met
- [ ] Error handling verified
- [ ] Favicon and OG image created
- [ ] Deployed to production hosting
- [ ] Share with ReFi DAO community

---

## 10. Appendix

### A. Reference Images
The following reference images define the visual style:
1. Portrait with orb background (woman with orange headband)
2. ReFi DAO logo with orb
3. "THE REGENERATIVE FINANCE DAO" branding
4. Color palette specification
5. Portrait with orb background (man with excited expression)

### B. remove.bg API Documentation
- API Reference: https://www.remove.bg/api
- API Endpoint: https://api.remove.bg/v1.0/removebg
- Free tier: 50 API calls per month

### C. Twitter Web Intent Documentation
- URL format: `https://twitter.com/intent/tweet?text={encoded_text}`
- Documentation: https://developer.twitter.com/en/docs/twitter-for-websites/tweet-button/guides/web-intent
