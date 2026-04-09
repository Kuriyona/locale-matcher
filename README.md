# Locale Matcher

`@kuriyona/locale-matcher`

[![npm](https://img.shields.io/npm/v/@kuriyona/locale-matcher)](https://www.npmjs.com/package/@kuriyona/locale-matcher)

[GitHub](https://github.com/kuriyona/locale-matcher)

A smart language tag matcher library for matching and sorting the most suitable language tags for users.

## Features

- Supports language-script-region matching (e.g. `zh-Hans-CN`)
- Priority-based intelligent sorting
- Built-in rules for common language variants (e.g. Chinese simplified/traditional, Cyrillic/Latin letters, etc.)
- 100% test coverage

## \<script\> + CDN

```html
<script src="https://cdn.jsdelivr.net/npm/@kuriyona/locale-matcher/dist/index.iife.js"></script>
<script src="https://unpkg.com/@kuriyona/locale-matcher/dist/index.iife.js"></script>
<script>
  const { match } = window.LocaleMatcher;
  console.log(match('zh-CN', ['zh-Hans', 'zh-Hant', 'en']));
</script>
```

## Install

```bash
npm install @kuriyona/locale-matcher
```

## Usage

```typescript
import { rank, match } from '@kuriyona/locale-matcher';

// Basic Match
match('zh-CN', ['zh-Hans', 'zh-Hant', 'en']);
// => ['zh-Hans', 'zh-Hant']

// Auto Detect Script
match('zh-TW', ['zh-Hans', 'zh-Hant', 'zh']);
// => ['zh-Hant', 'zh', 'zh-Hans']

// Multi-language
match('fr', ['fr-CA', 'fr-FR', 'es']);
// => ['fr-FR', 'fr-CA']

rank('zh-TW', ['zh-Hans', 'zh-Hant', 'zh']);
/*
[
  {
    language: 'zh',
    script: 'Hant',
    region: undefined,
    fullTag: 'zh-Hant',
    score: 1000,
    matchReason: {
      scriptMatched: true,
      regionMatched: false,
      isAffinitiveRegion: false
    }
  },
  {
    language: 'zh',
    script: undefined,
    region: undefined,
    fullTag: 'zh',
    score: 500,
    matchReason: {
      scriptMatched: false,
      regionMatched: false,
      isAffinitiveRegion: false
    }
  },
  ...
]
*/
```

## Match Rules

1. **Language Matching**: First filter candidates with the same language
2. **Script Matching** (highest priority):
   - Exact match: +1000 points
   - No script: +500 points
3. **Region Matching**:
   - Exact match: +500 points
   - Affinitive region match: +300 points
4. **Sorting Rules**:
   - Descending by total score
   - By built-in script priority (e.g., `Hans` > `Hant`)
   - By built-in region priority (e.g., `CN` > `TW` > `HK`)
   - Alphabetical order
