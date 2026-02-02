---
layout: default
title: Home
nav_order: 1
description: "Entry Web SDK - Production-ready SDK for secure biometric authentication"
permalink: /
---

# Entry Web SDK Documentation

Production-ready SDK for secure biometric authentication.
{: .fs-6 .fw-300 }

[Get Started](./integration){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 }
[View on GitHub Packages](https://github.com/Synapser-Limited/entry-web-sdk/pkgs/npm/entry-web-sdk){: .btn .fs-5 .mb-4 .mb-md-0 }

---

## Quick Links

| Documentation | Description |
|:--------------|:------------|
| 🚀 [Integration Guide](./integration) | Complete setup and integration instructions |
| 📋 [Changelog](./changelog) | Version history and release notes |
| 📄 [License](./license) | Software license terms |
| 🔒 [Security](./security) | Security guidelines and best practices |
| ⚠️ [Error Handling](./error-handling) | Error codes and handling strategies |
| 📖 [Use Cases](./use-cases) | Common integration scenarios |

---

## Installation

```bash
# Configure GitHub Packages auth (once)
echo "@synapser-limited:registry=https://npm.pkg.github.com" >> ~/.npmrc
echo "//npm.pkg.github.com/:_authToken=YOUR_GITHUB_TOKEN" >> ~/.npmrc

# Install the SDK
npm install @synapser-limited/entry-web-sdk
```

---

## Getting Started

```typescript
import { EntrySDK, EntryApiEnvironment, EntrySDKError } from '@synapser-limited/entry-web-sdk';

// Initialize SDK (provide your app name from Synapser)
const entrySDK = EntrySDK.getInstance(
  'your-app-name',
  EntryApiEnvironment.Live
);

// Identify user with biometric liveness check
async function authenticateUser() {
  try {
    const user = await entrySDK.identifyUser(
      true,  // Register if not found
      document.getElementById('auth-container')!
    );
    console.log('Authenticated:', user.entryUserId);
  } catch (error) {
    if (error instanceof EntrySDKError) {
      console.error(`Error ${error.code}: ${error.message}`);
    }
  }
}
```

**Requirements:**
- HTTPS domain (required for camera access)
- Container element for the authentication UI
- Valid app configuration from Synapser

For complete integration details, see the [Integration Guide](./integration).

---

## Support

For support inquiries, please contact [support@synapser.com](mailto:support@synapser.com).
