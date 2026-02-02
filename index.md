---
layout: default
title: Entry Web SDK
---

# Entry Web SDK Documentation

Production-ready SDK for secure biometric authentication.

## Quick Links

- 📋 [Changelog](./changelog) - Version history and release notes
- 📄 [License](./license) - Software license terms
- 📦 [Package](https://github.com/Synapser-Limited/entry-web-sdk/pkgs/npm/entry-web-sdk) - npm package on GitHub Packages

## Installation

```bash
# Configure GitHub Packages auth (once)
echo "@synapser-limited:registry=https://npm.pkg.github.com" >> ~/.npmrc
echo "//npm.pkg.github.com/:_authToken=YOUR_GITHUB_TOKEN" >> ~/.npmrc

# Install the SDK
npm install @synapser-limited/entry-web-sdk
```

## Getting Started

```typescript
import { EntrySDK, EntryApiEnvironment } from '@synapser-limited/entry-web-sdk';

// Initialize SDK
const entrySDK = EntrySDK.getInstance(
  'your-app-name',
  EntryApiEnvironment.Live
);
```

## Support

For support inquiries, please contact [support@synapser.com](mailto:support@synapser.com).
