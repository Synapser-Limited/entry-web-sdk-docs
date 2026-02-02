---
layout: default
title: SECURITY
---

# Entry Web SDK - Security and Compliance Guide

This document provides detailed information about security features, compliance considerations, and best practices for the Entry Web SDK.

## Security Features

### End-to-End Encryption

- AES-256-GCM encryption for all data transmission
- Dynamic key generation per session using PBKDF2
- Session-based authentication with daily key rotation
- Perfect forward secrecy with ephemeral session keys

### Biometric Security

- AWS Rekognition liveness detection prevents photo/video spoofing (iBeta Level 2 certified)
- Template-based storage (no raw biometric data stored)
- Configurable confidence thresholds (default 80%+ for liveness)
- Face matching with configurable similarity thresholds (95%+)

### Security Protection

- Device fingerprinting using browser characteristics
- Comprehensive audit logging and event tracking
- Session management with timeout and validation
- Request tracing with unique session identifiers
- Whitelisting of domains and IP addresses

### Audit & Compliance

- Complete audit trail
- Privacy-by-design architecture
- GDPR-ready features (user consent, data minimization, deletion)
- Configurable data retention policies

## Security Architecture

The Entry Web SDK implements robust security measures:

- **AWS Security Model** - Leverages AWS Cognito and Rekognition security infrastructure
- **Data Protection** - No sensitive biometric data stored in browser or transmitted
- **Access Control** - Role-based authentication with session management
- **Audit Trail** - Comprehensive logging via Sentry and custom analytics

## Compliance Considerations

### GDPR Compliance

The SDK supports GDPR requirements through:

- **User Consent**: Clear consent mechanisms for biometric data collection
- **Data Minimization**: Only essential data is collected and stored
- **Right to Deletion**: `deleteUser()` method for complete data removal
- **Data Portability**: User data can be exported in standard formats
- **Privacy by Design**: Built-in privacy protections

### Industry-Specific Compliance

#### Healthcare (HIPAA)

- Audit logging for all authentication events
- Secure session management
- No PHI stored in SDK
- Integration with existing healthcare compliance systems

#### Financial Services (PCI DSS, SOC 2)

- Strong authentication for high-value transactions
- Fraud detection integration points
- Comprehensive audit trails
- Session security and timeout management

#### Age-Restricted Content (COPPA)

- Age verification through biometric-linked identity documents
- Automatic flagging and restrictions for underage users
- Privacy-compliant data handling
- Regional compliance support (16+ for EU, 18+ for alcohol/gambling)

## Best Practices

### Production Deployment

1. **Environment Configuration**
   - Use `EntryApiEnvironment.Production` for production
   - Configure proper domain whitelisting
   - Set up appropriate error monitoring

2. **Error Handling**
   - Implement comprehensive error handling
   - Provide clear user feedback for common errors
   - Log errors for monitoring and debugging

3. **Performance Optimization**
   - Implement retry logic with exponential backoff
   - Cache SDK instance appropriately
   - Monitor authentication success rates

4. **Security Hardening**
   - Enforce HTTPS for all requests
   - Implement Content Security Policy (CSP)
   - Regular security audits
   - Monitor for suspicious authentication patterns

### Data Protection

1. **No Local Storage of Biometric Data**
   - SDK never stores raw biometric data locally
   - Only templates are stored in secure backend
   - Browser data cleared on session end

2. **Session Management**
   - Implement proper session timeouts
   - Clear sensitive data on logout
   - Validate sessions on each request

3. **Audit Logging**
   - Log all authentication attempts
   - Track success/failure rates
   - Monitor for anomalous patterns
   - Retain logs per compliance requirements

## Security Testing

### Recommended Testing Scenarios

1. **Authentication Security**
   - Test liveness detection with photos/videos
   - Verify similarity threshold enforcement
   - Test device fingerprinting
   - Validate session management

2. **Network Security**
   - Verify HTTPS enforcement
   - Test with network interruptions
   - Validate error handling for network issues

3. **Data Protection**
   - Verify no biometric data in browser storage
   - Check data transmission encryption
   - Validate data deletion

4. **Access Control**
   - Test unauthorized access attempts
   - Verify session expiration
   - Validate domain whitelisting

## Incident Response

### Security Incident Handling

1. **Detection**
   - Monitor error rates for suspicious patterns
   - Track failed authentication attempts
   - Alert on anomalous behavior

2. **Response**
   - Review audit logs
   - Identify affected users
   - Implement countermeasures

3. **Recovery**
   - Reset affected sessions
   - Update security configurations
   - Communicate with affected users

4. **Post-Incident**
   - Document incident details
   - Update security procedures
   - Implement preventive measures
