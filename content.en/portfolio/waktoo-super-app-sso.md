+++
date = '2025-09-10T23:11:28+07:00'
draft = false
title = 'Waktoo Super App - Unified Authentication Across Multiple App'
featured_image = "/blog/images/portfolio/waktoo-superapp-auth-sso-flow.png"
tags = ["portfolio", "backend", "php", "laravel", "auth"]
+++

Implemented a JWT-based authentication system that unified three previously independent applications
into the Waktoo Super App, enabling seamless Single Sign-On (SSO) while preserving existing API authentication flows.

<!--more-->

## Overview

At Waktoo (part of Kazee), we developed three SASS products, **Waktoo CRM**, **Waktoo HRM**, and **Waktoo Commerce**.  

Later, we began building the Waktoo Super App -
a unified platform that brings all three products into a single seamless experience.

One of the biggest technical challenge was authentication. Each product had its own JWT-based API authentication system. 
We needed a centralized authentication system that worked across all products without breaking existing authentication flows.

## The Problems

- Users had to log in separately for each product.
- Each product generated its own JWT, which wasn’t valid elsewhere.
- Replacing old auth systems outright was too risky since existing clients relied on them.
- We needed Single Sign-On (SSO), but also backward compatibility.

## The Solutions

I designed and implemented the SuperApp Authentication Service, which introduced:
- Single Login. Users authenticate once through the Super App portal.
- Unified Token. A JWT that works across CRM, HRM, and Commerce APIs.
- Backward Compatibility. legacy JWTs from each product remain valid.
- Seamless Integration. no breaking changes for existing clients.

This solution provided the foundation for the Super App — unifying authentication without disrupting existing products.


## Architecture & Flows

![Authentication Flow Diagram](/blog/images/portfolio/waktoo-superapp-auth-sso-flow.png)

The SuperApp Authentication Service issues JWTs signed with RS256 (asymmetric encryption).
Each service validates tokens using only the public key, enabling independent verification without relying on network calls to the issuer.

Here’s how the flow works in practice:
1. **Login** - Users authenticate through the Super App portal.
2. **Token Issuance** - On success, the system issues a signed JWT containing identity and claims (e.g., sub, roles, exp).
3. **Token Usage** - Clients attach the token to API requests using the Bearer scheme.
4. **Verification** - Each product verifies the JWT signature with the public key and checks validity (expiry, claims, etc.).
5. **Access** - If valid, access is granted; otherwise, the request is rejected.

![Waktoo Super App Token](/blog/images/portfolio/wakto-superapp-auth-token.png)

## Impact

By consolidating authentication into a JWT-based SSO system, we solved the problem of fragmented sessions and inconsistent login flows.

Key improvements:
- User Experience. Seamless login across all products.
- Performance. Faster validation thanks to local public key checks (no network round trips).
- Security. Stronger signing using private–public key pairs.
- Maintainability. Reduced duplicate auth logic across services.
- Scalability. A solid foundation for future products in the Waktoo ecosystem.

