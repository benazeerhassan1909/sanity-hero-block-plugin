# Sanity Plugin Hero Block

> A comprehensive and customizable hero block plugin for **Sanity Studio v3** with support for images, videos, overlays, and call-to-action buttons.


## Features

📝 **Content Management**
- Heading and subheading text fields
- Content alignment options (left, center, right)
- Customizable text colors

🔘 **Call-to-Action Button**
- Optional button with custom text
- Link support (internal and external URLs)
- Open in new tab option

🎨 **Styling & Background Options**
- **Background Type**: Choose between color or media
  - **Color**: Hex color picker for solid backgrounds
  - **Media**: Choose between image or video
    - **Image**: Upload images through Sanity asset integration
    - **Video**: Two video options
      - **Upload**: Upload video files (MP4, WebM, OGG support)
      - **URL**: Embed external videos (YouTube, Vimeo, direct URLs)
- **Overlay Settings**:
  - Color overlay for media backgrounds
  - Adjustable opacity settings
- **Styling Options**
  - Button background and text color customization
  - Content text color options

📱 **Responsive Design**
- Mobile-first responsive layout
- Optimized for all device sizes

🔧 **Developer Features**
- TypeScript support with full type definitions
- Self-contained styling with styled-components
- Sanity v3/v4 compatible
- Comprehensive error handling and asset fallbacks

## Installation

```sh
npm install @multidots/sanity-plugin-hero-block
```

## Setup

### 1. Add Plugin to Sanity Config

Add it as a plugin in `sanity.config.ts` (or .js):

```ts
import {defineConfig} from 'sanity'
import {heroBlockPlugin} from '@multidots/sanity-plugin-hero-block'

export default defineConfig({
  // ... other config
  plugins: [
    heroBlockPlugin(),
    // ... other plugins
  ],
})
```

### 2. Add to Your Schema

Include the hero block in your page or document schemas:

```ts
// In your page schema
import {heroBlockType} from '@multidots/sanity-plugin-hero-block'

export const pageSchema = defineType({
  name: 'page',
  type: 'document',
  fields: [
    defineField({
            name: "heroBlock",
            type: "heroBlock",
        }),
  ]
})
```

### 3. Update GROQ Queries

Ensure your GROQ queries properly expand asset references:

```groq
*[_type == "page" && slug.current == $slug][0]{
  content[]{
    ...,
    _type == "heroBlock" => {
      ...,
      "heroImage": heroImage{
        ...,
        asset->{
          _id,
          url
        }
      },
      heroVideoType,
      heroVideoUrl,
      "heroVideo": heroVideo{
        ...,
        asset->{
          _id,
          url
        }
      }
    }
  }
}
```

### 4. Frontend Component Usage

```tsx
import { HeroBlock } from "@multidots/sanity-plugin-hero-block";
import { urlFor } from "@/sanity/lib/image";
import { projectId, dataset } from "@/sanity/env";

export default function HeroBlockComponent({ heroBlock }) {
  return (
    <HeroBlock 
      heroBlock={heroBlock} 
      urlFor={urlFor}           // For image asset processing
      projectId={projectId}     // For video asset URL construction
      dataset={dataset}         // For video asset URL construction
    />
  );
}
```
## Available Fields

### Content
- **Heading Text** - Main hero title
- **Subheading** - Secondary text below the heading
- **Content Alignment** - Left, center, or right alignment
- **Text Color** - Custom color for text content

### Call-to-Action Button
- **Button Text** - Custom button label
- **Button Link** - Internal or external URL
- **Open in New Tab** - Option for external links
- **Button Colors** - Background and text color customization

### Background Options
- **Background Type** - Choose between solid color or media
- **Background Color** - Hex color picker for solid backgrounds
- **Background Media** - Upload images or videos
- **Video Source** - Upload files or use external URLs (YouTube, Vimeo)

### Overlay Settings
- **Overlay Color** - Color overlay for media backgrounds
- **Overlay Opacity** - Transparency control (0-100%)

## Schema Fields

### Content Fields
- **`headingText`** (string, required) - Main hero heading
- **`subHeading`** (string, required) - Secondary text below heading

### Call to Action
- **`callToActionText`** (string, optional) - Button text
- **`callToActionLink`** (url, optional) - Button destination URL
- **`openInNewTab`** (boolean, optional) - Whether link opens in new tab

### Background Settings
- **`heroBgType`** (string) - Background type: `"color"` or `"media"`
- **`heroBgColor`** (color) - Background color (when type is "color")
- **`heroBgMedia`** (string) - Media type: `"image"` or `"video"` (when type is "media")
- **`heroImage`** (image) - Background image asset
- **`heroVideoType`** (string) - Video source type: `"url"` or `"upload"` (when media type is "video")
- **`heroVideoUrl`** (url) - Video URL for external videos like YouTube, Vimeo, or direct video links (when video type is "url")
- **`heroVideo`** (file) - Background video asset for uploaded files (when video type is "upload", accepts video/* formats)

### Overlay Settings (for media backgrounds)
- **`heroOverlayColor`** (color) - Overlay color
- **`heroOverlayOpacity`** (number) - Overlay opacity (0-1)

### Styling Options
- **`contentAlignment`** (string) - Text alignment: `"left"`, `"center"`, or `"right"`
- **`contentColor`** (color) - Text color for heading and subheading
- **`buttonBgColor`** (color) - Call-to-action button background color
- **`buttonTextColor`** (color) - Call-to-action button text color

## Component API

### Props

```tsx
interface HeroBlockProps {
  heroBlock: HeroBlockType;
  urlFor?: (source: any) => ImageUrlBuilder;  // Sanity image URL builder
  projectId?: string;                         // Sanity project ID (for video URLs)
  dataset?: string;                          // Sanity dataset (for video URLs)
}
```

### HeroBlockType Interface

```tsx
interface HeroBlockType {
  headingText: string;
  subHeading: string;
  callToAction?: {
    callToActionText?: string;
    callToActionLink?: string;
    openInNewTab?: boolean;
  };
  heroBgType: 'color' | 'media';
  heroBgColor?: { hex: string };
  heroBgMedia?: 'image' | 'video';
  heroImage?: {
    asset: {
      _ref: string;
      _type: 'reference';
      url?: string;
    };
    alt?: string;
  };
  heroVideoType?: 'url' | 'upload';
  heroVideoUrl?: string;
  heroVideo?: {
    asset: {
      _ref: string;
      _type: 'reference';
      url?: string;
    };
  };
  heroOverlay?: {
    heroOverlayColor?: { hex: string };
    heroOverlayOpacity?: number;
  };
  contentAlignment: 'left' | 'center' | 'right';
  contentColor?: { hex: string };
  buttonBgColor?: { hex: string };
  buttonTextColor?: { hex: string };
}
```

## Screenshots

![Hero Block Backend Settings](https://share.cleanshot.com/YF5ssx62Yf86VJsv2BM8)

![Hero Block Style Settings](https://share.cleanshot.com/dBmPq0Mz52tbZRFTnZmR)

![Hero Block Frontend Color BG](https://share.cleanshot.com/Kjh7DXny2tHfqrV1XD3s)

![Hero Block Frontend Image BG](https://share.cleanshot.com/YYrJjS8fMwp2xY024zFy)

![Hero Block Frontend Video BG](https://share.cleanshot.com/1p6RgSfgVzcq61QTHcRl)


## Demo Video



## Troubleshooting

### Video Not Playing

#### For Uploaded Videos:
1. Ensure your GROQ query expands video assets: `asset->{ url }`
2. Check that video files are in web-compatible formats (MP4, WebM, OGG)
3. Verify `projectId` and `dataset` props are provided for fallback URL construction
4. Ensure `heroVideoType` is set to `"upload"`
5. **Mobile-specific**: Check that videos are optimized for mobile bandwidth
6. **Autoplay issues**: Mobile browsers may block autoplay - ensure videos are muted

#### For Video URLs (YouTube/Vimeo):
1. Verify the video URL is publicly accessible
2. Check that `heroVideoType` is set to `"url"`
3. Ensure `heroVideoUrl` contains a valid URL
4. For YouTube: Use standard watch URLs (`https://www.youtube.com/watch?v=...`)
5. For Vimeo: Use standard video URLs (`https://vimeo.com/...`)
6. Some videos may not allow embedding - check video privacy settings

### Images Not Loading
1. Ensure `urlFor` helper is passed to the component
2. Verify your GROQ query expands image assets: `asset->{ url }`
3. Check that images are properly uploaded to Sanity


## Development

This plugin uses [@sanity/plugin-kit](https://github.com/sanity-io/plugin-kit) with default configuration for build & watch scripts.

### Testing in Studio
See [Testing a plugin in Sanity Studio](https://github.com/sanity-io/plugin-kit#testing-a-plugin-in-sanity-studio) for hot-reload development setup.

### Building
```sh
npm run build
```

### Development with Watch
```sh
npm run watch
```

## License

[MIT](LICENSE) © Multidots

## Support

For issues, feature requests, or questions, please create an issue in the repository or contact [sanity@multidots.com](mailto:sanity@multidots.com).
