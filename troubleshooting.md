---
layout: default
title: Troubleshooting
nav_order: 8
description: "Common issues, FAQs, and debugging tips for Entry Web SDK"
---

# Troubleshooting & FAQ

Common issues and solutions for Entry Web SDK integration.
{: .fs-6 .fw-300 }

---

## Quick Diagnostics

{: .important }
> Before troubleshooting, ensure you have:
>
> - HTTPS enabled (required for camera access)
> - A valid app name registered with Synapser
> - A supported browser (Chrome 80+, Firefox 75+, Safari 13+, Edge 80+)

---

## Common Issues

### Camera Not Working

{: .warning }
> The SDK requires HTTPS. Camera access will not work on `http://` URLs (except `localhost`).

**Symptoms:**

- `CAMERA_ACCESS_DENIED` error
- Black screen where camera should appear
- Browser doesn't prompt for camera permission

**Solutions:**

1. **Check HTTPS** - Ensure your site uses `https://`
2. **Check permissions** - Click the lock icon in the address bar → Site settings → Camera → Allow
3. **Check other apps** - Close other applications using the camera
4. **Try incognito mode** - Extensions can block camera access

```typescript
// Check camera availability before calling SDK
async function checkCameraAvailable(): Promise<boolean> {
  try {
    const stream = await navigator.mediaDevices.getUserMedia({ video: true });
    stream.getTracks().forEach(track => track.stop());
    return true;
  } catch (e) {
    return false;
  }
}
```

---

### Liveness Check Failing

**Symptoms:**

- `LIVENESS_CHECK_FAILED` error
- User passes camera check but fails liveness

**Solutions:**

1. **Improve lighting** - Face should be evenly lit, avoid backlighting
2. **Remove obstructions** - Remove glasses, hats, masks if possible
3. **Steady position** - Keep face centered and still
4. **Clean camera lens** - Wipe camera lens clean

{: .note }
> The liveness check uses AWS Rekognition which is iBeta Level 2 certified. It's designed to reject photos and videos of faces.

---

### Network Errors

**Symptoms:**

- `NETWORK_ERROR` or `TIMEOUT_ERROR`
- SDK hangs during initialization

**Solutions:**

1. **Check connectivity** - Verify internet connection
2. **Check firewall** - Ensure these domains are accessible:
   - `*.amazonaws.com`
   - `*.synapser.com`
3. **Retry with backoff** - Implement exponential backoff

```typescript
// Network check before SDK initialization
if (!navigator.onLine) {
  showError('Please check your internet connection');
  return;
}
```

---

### Invalid Configuration

**Symptoms:**

- `INVALID_CONFIGURATION` or `INVALID_APP_NAME` error
- SDK fails during `getInstance()`

**Solutions:**

1. **Verify app name** - Must match exactly what's registered with Synapser
2. **Check environment** - Use correct environment (`Test`, `Demo`, `Live`)
3. **Check domain whitelist** - Your domain must be whitelisted by Synapser

{: .highlight }
> Contact [support@synapser.com](mailto:support@synapser.com) to verify your app configuration.

---

### User Not Found

**Symptoms:**

- `USER_NOT_FOUND` error when `registerIfNotFound` is `false`

**Solution:**

This is expected behavior when the user hasn't registered. Either:

- Set `registerIfNotFound: true` to allow registration
- Direct users to a registration flow first

```typescript
// Handle user not found gracefully
try {
  const user = await sdk.identifyUser(false, container);
} catch (error) {
  if (error instanceof EntrySDKError && error.code === 'USER_NOT_FOUND') {
    // Redirect to registration or enable registration
    const user = await sdk.identifyUser(true, container);
  }
}
```

---

## Framework-Specific Issues

### React: Component Unmounts During Flow

**Problem:** SDK UI disappears unexpectedly

**Solution:** Ensure the container element persists during the authentication flow:

```tsx
// ❌ Wrong - container may unmount
{showAuth && <div id="auth-container" />}

// ✅ Correct - container always exists
<div id="auth-container" style={{ display: showAuth ? 'block' : 'none' }} />
```

### React: Multiple SDK Instances

**Problem:** `SDK already initialized with app 'X'` error

**Solution:** The SDK is a singleton. Don't call `getInstance()` with different app names:

```typescript
// ❌ Wrong
const sdk1 = EntrySDK.getInstance('app1', env);
const sdk2 = EntrySDK.getInstance('app2', env); // Error!

// ✅ Correct - use same app name or reset first
EntrySDK.reset();
const sdk = EntrySDK.getInstance('app2', env);
```

### Vue: Reactivity Issues

**Problem:** User data not updating in Vue components

**Solution:** Ensure you're using Vue's reactivity system:

```vue
<script setup>
import { ref } from 'vue';

const user = ref(null);

async function authenticate() {
  const result = await sdk.identifyUser(true, container);
  user.value = result; // Triggers reactivity
}
</script>
```

---

## Debugging Tips

### Enable Debug Logging

```typescript
const sdk = EntrySDK.getInstance('your-app', EntryApiEnvironment.Test, {
  enableDebugLogging: true
});
```

### Check Error Details

```typescript
catch (error) {
  if (error instanceof EntrySDKError) {
    console.log('Code:', error.code);
    console.log('Message:', error.message);
    console.log('Context:', error.context);
    console.log('Cause:', error.cause);
    console.log('Timestamp:', error.timestamp);
    console.log('Retryable:', error.isRetryable());
  }
}
```

### Browser DevTools

1. **Console** - Check for SDK error messages
2. **Network** - Verify API calls are succeeding
3. **Application** - Check sessionStorage for `browserSessionId`

---

## FAQ

### Is HTTPS required?

{: .important }
> **Yes.** Camera access requires a secure context (HTTPS). The only exception is `localhost` for development.

### Which browsers are supported?

| Browser | Minimum Version |
|---------|-----------------|
| Chrome | 80+ |
| Firefox | 75+ |
| Safari | 13+ |
| Edge | 80+ |

### Can I use the SDK in a mobile app?

The Web SDK is for browser-based applications. For native mobile apps, contact Synapser about:

- Entry iOS SDK
- Entry Android SDK

### How do I handle users on unsupported devices?

```typescript
try {
  const user = await sdk.identifyUser(true, container);
} catch (error) {
  if (error.code === 'DEVICE_NOT_SUPPORTED' || error.code === 'BROWSER_NOT_SUPPORTED') {
    showFallbackAuth(); // Show alternative authentication
  }
}
```

### Can users register on multiple devices?

Yes. Each device gets a unique `deviceId`. The same user can authenticate from multiple devices.

### How do I delete a user's data (GDPR)?

```typescript
await sdk.deleteUser(user.entryUserId);
```

{: .note }
> This permanently removes the user's biometric template and profile data.

---

## Still Need Help?

Contact [support@synapser.com](mailto:support@synapser.com) with:

- SDK version
- Browser and version
- Error code and message
- Steps to reproduce
