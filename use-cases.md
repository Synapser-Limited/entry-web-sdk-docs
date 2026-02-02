---
layout: default
title: USE CASES
---

# Entry Web SDK - Use Cases and Implementation Patterns

This document describes common use cases and implementation patterns for the Entry Web SDK.

## Primary Use Cases

- **Secure User Onboarding**: Replace traditional password-based registration with biometric enrollment
- **Passwordless Authentication**: Enable frictionless login experience
- **Identity Verification**: Verify user identity for high-security transactions

## Industry-Specific Applications

### 1. Account Recovery and Reset

**Scenario**: Users forget passwords or lose access to traditional MFA devices

**Implementation**: Banking applications can leverage biometric-based recovery to reset access without emailing potentially vulnerable reset links, significantly reducing phishing risks.

**Technical Integration**:

- Utilizes template-based biometric storage for secure identity verification
- Configurable confidence thresholds ensure high-security recovery processes
- Eliminates dependency on email-based recovery flows

**Example**: A mobile banking app allows users to regain account access by completing facial verification, bypassing traditional SMS or email recovery methods.

### 2. Multi-Device Synchronization

**Scenario**: Users switching between devices (desktop to mobile web) requiring seamless session transfer

**Implementation**: E-learning platforms can maintain logged-in states across browsers through biometric re-authentication while enforcing device fingerprinting to prevent unauthorized access.

**Technical Integration**:

- Session-based authentication with daily key rotation
- Device fingerprinting for cross-device security validation
- Seamless user experience with continuous authentication state

**Example**: A student starts a course on their laptop and continues on their mobile device, with the SDK automatically re-authenticating via facial recognition.

### 3. High-Compliance Environments (Healthcare & Finance)

**Scenario**: Regulated sectors requiring KYC (Know Your Customer) processes for sensitive data access

**Implementation**: Healthcare portals and financial services can verify user identity against stored biometric templates during critical actions like accessing medical records or approving loan applications.

**Technical Integration**:

- AWS Rekognition liveness detection (iBeta Level 2 certified)
- Comprehensive audit logging for compliance requirements
- Template-based verification without storing raw biometric data

**Example**: A telehealth platform requires facial verification before displaying patient medical records, ensuring HIPAA compliance and audit trail documentation.

### 4. E-Commerce Fraud Prevention

**Scenario**: Real-time identity verification during checkout to flag suspicious transactions

**Implementation**: Online retailers can integrate biometric verification to confirm the user's identity matches the account holder's enrolled template, particularly for transactions from new devices.

**Technical Integration**:

- Session-based authentication with device anomaly detection
- Configurable similarity thresholds for transaction verification
- Integration with existing fraud detection systems

**Example**: An e-commerce site prompts facial verification for high-value purchases from unrecognized devices, reducing chargeback rates and fraudulent transactions.

### 5. Remote Workforce Access

**Scenario**: Enterprise VPN or collaboration tools requiring secure passwordless access for remote employees

**Implementation**: Corporate applications can enable secure access from varying locations while incorporating IP whitelisting and encryption key rotation.

**Technical Integration**:

- Domain and IP address whitelisting for corporate security
- Dynamic key generation with daily rotation cycles
- Fallback authentication when biometric confidence falls below thresholds

**Example**: A remote employee accesses company resources via facial recognition, with the system automatically adjusting security parameters based on connection location and environmental factors.

### 6. Age Verification for Restricted Content

**Scenario**: Platforms requiring age verification for compliance with regulatory requirements such as COPPA, age-gating regulations, and jurisdiction-specific restrictions

**Implementation**: Gaming platforms, alcohol e-commerce, or adult content sites can verify user age through biometric-linked identity documents stored in the EntryUser profile, ensuring compliance with laws like COPPA (Children's Online Privacy Protection Act) and regional age-restriction mandates.

**Technical Integration**:

- Integration with photo identity document data (passport, ID card, driver's license)
- Cross-reference `dateOfBirth` field in EntryUser with `photoIdentityDocumentNumber` for verification
- Liveness detection to prevent document spoofing and synthetic identity fraud
- Privacy-compliant data handling with automatic data minimization for underage users
- Audit trail generation for regulatory compliance reporting

**Compliance Features**:

- **COPPA compliance**: Automatic flagging and data handling restrictions for users under 13
- **Regional compliance**: Support for varying age thresholds (16+ for EU GDPR, 18+ for alcohol/gambling)
- **Document validation**: Verify document authenticity through government ID cross-referencing

**Example**: An online gaming platform verifies a user's age by cross-referencing their biometric template with stored identity document information, automatically restricting access for users under the required age threshold and maintaining compliance audit trails.

## Technical Implementation Patterns

### Authentication Flow Integration

```typescript
// Example: High-security transaction verification
async function verifyHighValueTransaction(transactionAmount: number) {
  try {
    // Step 1: Check if transaction requires additional verification
    if (transactionAmount > THRESHOLD_AMOUNT) {
      const overlayElement = document.getElementById('verification-overlay');
      
      // Step 2: Perform biometric verification
      const user = await entrySDK.identifyUser(false, overlayElement);
      
      // Step 3: Validate against stored user profile
      if (user.isRegistrationComplete) {
        return {
          verified: true,
          userId: user.entryUserId,
          confidence: 'high',
          auditTrail: generateAuditLog(user, 'transaction_verification')
        };
      }
    }
    
    return { verified: false, reason: 'verification_required' };
  } catch (error) {
    // Handle specific error scenarios
    return handleVerificationError(error);
  }
}
```

### Cross-Device Session Management

```typescript
// Example: Multi-device synchronization
async function synchronizeDeviceSession(deviceId: string) {
  try {
    const overlayElement = document.getElementById('sync-overlay');
    const user = await entrySDK.identifyUser(false, overlayElement);
    
    // Validate device fingerprint match
    if (user.deviceId !== deviceId) {
      // Prompt for device registration or use fallback auth
      return await handleDeviceMismatch(user, deviceId);
    }
    
    return {
      sessionId: generateSessionId(),
      user: user,
      deviceVerified: true
    };
  } catch (error) {
    return await fallbackAuthentication(error);
  }
}
```
