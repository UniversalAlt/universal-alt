---
layout: layouts/post.njk
title: "Accessible Content Description System (ACDS), including new IPTC meta fields for ALT descriptions."
description: Draft specifications
date: 2024-10-13
tags:
  - accessibility
---

# Accessible Content Description System (ACDS), including new IPTC meta fields for ALT descriptions.

## ACDS Specification

### 1. Protocol

The ACDS protocol is designed for efficient distribution of ALT descriptions across the internet.

#### 1.1 Transport Layer
- Primary: UDP for quick lookups
- Secondary: TCP for larger transfers

#### 1.2 Message Types
1. Query: Request ALT descriptions
2. Response: Contain ALT descriptions
3. Update: Add or modify descriptions
4. Synchronization: Keep distributed servers up-to-date

#### 1.3 Message Format
```
Header:
  - Message ID (16 bits)
  - Flags (16 bits)
  - Question Count (16 bits)
  - Answer Count (16 bits)
  - Authority Count (16 bits)
  - Additional Count (16 bits)

Question Section:
  - URL (variable length)
  - Type (16 bits)
  - Class (16 bits)

Answer Section:
  - URL (variable length)
  - Type (16 bits)
  - Class (16 bits)
  - TTL (32 bits)
  - Data Length (16 bits)
  - ALT Description (variable length)
```

### 2. Server Architecture

#### 2.1 Server Types
1. Root ACDS Servers
2. Top-Level Domain (TLD) ACDS Servers
3. Authoritative ACDS Servers
4. Recursive ACDS Servers

#### 2.2 Server Distribution
- Minimum of 13 root servers, geographically distributed
- TLD servers for each top-level domain
- Authoritative servers managed by domain owners or service providers

### 3. ACDS Records

#### 3.1 Record Types
1. A (ALT): Contains the primary ALT description
2. AA (Additional ALT): Contains additional or language-specific ALT descriptions
3. M (Metadata): Contains metadata about the image or media

#### 3.2 Record Format
```
Record:
  - URL (variable length)
  - Type (8 bits)
  - Class (16 bits)
  - TTL (32 bits)
  - Data Length (16 bits)
  - Data (variable length)
```

### 4. ACDS Resolvers

#### 4.1 Integration Points
- Web browsers
- Content Management Systems (CMSes)
- Screen readers
- Search engine crawlers

#### 4.2 Resolver Functions
- Query ACDS servers
- Cache responses
- Update local cache
- Handle timeouts and errors

### 5. Security

#### 5.1 Authentication
- TSIG (Transaction Signature) for server-to-server communication
- DNSSEC-like system for origin authentication and data integrity

#### 5.2 Encryption
- TLS for all TCP connections
- DTLS for UDP connections where supported

### 6. IPTC Meta Fields for ALT Descriptions

To support the ACDS, we propose the following new IPTC meta fields:

#### 6.1 ACDS Fields
1. ACDS:ALTDescription
	- Type: String
	- Description: Primary ALT description for the image

2. ACDS:AdditionalALT
	- Type: Array of Strings
	- Description: Additional ALT descriptions, including language-specific versions

3. ACDS:ALTLanguage
	- Type: String
	- Description: Language code for the primary ALT description (e.g., "en-US")

4. ACDS:ALTCreator
	- Type: String
	- Description: Entity responsible for creating the ALT description

5. ACDS:ALTCreationDate
	- Type: Date
	- Description: Date when the ALT description was created or last updated

6. ACDS:ALTVersion
	- Type: Integer
	- Description: Version number of the ALT description

7. ACDS:ContentType
	- Type: String
	- Description: Type of content (e.g., "image", "video", "audio")

8. ACDS:ContentContext
	- Type: String
	- Description: Context in which the content appears (e.g., "article", "product", "advertisement")

#### 6.2 Example Usage
```xml
<iptc:ACDS:ALTDescription>A golden retriever puppy sitting in a field of daisies</iptc:ACDS:ALTDescription>
<iptc:ACDS:AdditionalALT>
  <rdf:Bag>
    <rdf:li xml:lang="es">Un cachorro de golden retriever sentado en un campo de margaritas</rdf:li>
    <rdf:li xml:lang="fr">Un chiot golden retriever assis dans un champ de marguerites</rdf:li>
  </rdf:Bag>
</iptc:ACDS:AdditionalALT>
<iptc:ACDS:ALTLanguage>en-US</iptc:ACDS:ALTLanguage>
<iptc:ACDS:ALTCreator>ACDS AI Generator</iptc:ACDS:ALTCreator>
<iptc:ACDS:ALTCreationDate>2024-10-13</iptc:ACDS:ALTCreationDate>
<iptc:ACDS:ALTVersion>1</iptc:ACDS:ALTVersion>
<iptc:ACDS:ContentType>image</iptc:ACDS:ContentType>
<iptc:ACDS:ContentContext>article</iptc:ACDS:ContentContext>
```

This specification outlines the core components of the ACDS, including the protocol, server architecture, record types, and new IPTC meta fields for ALT descriptions. It provides a framework for implementing a distributed system for managing and delivering accessible content descriptions across the internet.

