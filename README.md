# JUREON: Where Every Citation Comes with a Chain of Custody while Encoding Certainty.
What remains protected:
JUREON™ trademark and logos
Proprietary verification algorithms
Certainty scoring formulas
Training data and models
".jvf Verified" certification marks

WHY WE'RE DOING THIS
Legal verification should be an open standard.

GETTING STARTED
Download the specification:
https://specs.jureon.com/jvf-v1.0
Join the consortium: 
https://consortium.jureon.com

Access developer tools:
https://dev.jureon.com

CERTIFICATION PATH
While anyone can create .jvf files, only JUREON-certified tools may display the ".jvf Verified" seal. Certification ensures:
Compliance with verification standards
Security and privacy requirements
Audit trail integrity

1. FILE OVERVIEW
1.1 Purpose
The .jvf format provides a standardized container for verifiable legal arguments with complete chain-of-custody tracking, enabling deterministic legal verification across platforms.
1.2 Core Principles
Self-contained: All references include full text
Verifiable: Cryptographic integrity checks
Temporally anchored: Legal validity at creation time
Deterministic: Same input → same verification output
Portable: Platform-independent ZIP-based format
1.3 Compliance Levels
BASIC: Valid structure, no verification guarantees
STANDARD: Automated verification passed
CERTIFIED: Human-reviewed, temporally anchored
2. TECHNICAL SPECIFICATION
2.1 Container Format
.jvf files are ZIP 2.0 archives with the following requirements:


File Structure:
.jvf (ZIP root)
├── META-INF/
│   ├── manifest.json           # REQUIRED - Primary manifest
│   └── signatures/             # OPTIONAL - Digital signatures
│
├── CONTENT/
│   ├── authorities/            # REQUIRED - Legal authorities
│   ├── logic/                  # REQUIRED - Logical structure
│   ├── metadata/               # REQUIRED - Provenance data
│   └── evidence/               # OPTIONAL - Supporting evidence
│
└── PROOFS/
    ├── verification.json       # REQUIRED - Verification results
    ├── certainty-scores.json   # REQUIRED - Certainty metrics
    └── audit-trail.json        # REQUIRED - Chain of custody

2.2 File Naming Rules
Lowercase with hyphens: usc-title11-507.json
No spaces or special characters except hyphen/underscore
Maximum 255 bytes UTF-8
JSON files must be valid RFC 8259 JSON

2.3 MIME Type
application/vnd.jureon.verification+json

3. REQUIRED FILES SPECIFICATION
3.1 META-INF/manifest.json

4. JSON SCHEMAS
4.1 Schema Validation
All JSON files must validate against their respective schemas. Core schemas:

Manifest Schema: Defines file structure and integrity

Authority Schema: Standardizes legal authority representation

Logic Schema: Defines argument structure and Hohfeldian relations

Proof Schema: Standardizes verification and certainty data

4.2 Schema Location Convention
text
https://specs.jureon.com/schemas/{type}/{subtype}/v{major}.{minor}.json
Example: https://specs.jureon.com/schemas/authority/case/v1.0.json

5. SECURITY AND INTEGRITY
5.1 Hash Algorithms
Primary: SHA3-384 (recommended)

Acceptable: SHA-256

Deprecated: SHA-1 (allowed for compatibility only)

5.2 Digital Signatures
Supported formats:

X.509 certificates with PKCS#7 signatures

JSON Web Signatures (JWS) with RFC 7515 compliance

PQC signatures (post-quantum cryptography ready)

5.3 Timestamping
RFC 3161 compliant timestamps required for CERTIFIED level

Multiple timestamp authorities supported for redundancy

Timestamp tokens stored in META-INF/signatures/

6. VERSIONING POLICY
6.1 Semantic Versioning
MAJOR: Breaking changes to structure or schemas

MINOR: New optional features, backward compatible

PATCH: Bug fixes, clarifications, non-breaking

6.2 Backward Compatibility
v1.x files must be readable by v1.0 validators

New fields may be added as optional

No removal of required fields within major version

6.3 Deprecation Process
Feature marked deprecated in release notes

Continues to work for next major version

Removed in next major version

7. IMPLEMENTATION REQUIREMENTS
7.1 Minimum Validator Requirements
A compliant .jvf validator must:

Validate ZIP structure

Check all required files exist

Validate JSON against schemas

Verify hashes in manifest

Check digital signatures (if present)

7.2 Reference Implementation
Open-source reference implementation provided at:

text
https://github.com/jureon-standards/jvf-validator
7.3 Test Suite
Compliance test suite available:

text
https://github.com/jureon-standards/jvf-compliance-tests
8. EXTENSION MECHANISMS
8.1 Custom Extensions
Extensions may be added in:

text
EXTENSIONS/
└── {vendor-id}/
    └── {extension-name}.json
Example: EXTENSIONS/com.microsoft/custom-fields.json

8.2 Vendor Prefix Requirement
Use reverse DNS notation: com.company

Must register prefix with Jureon Consortium

No conflict with reserved namespaces

8.3 Reserved Namespaces
org.jureon – Consortium extensions

com.example – Example/test extensions

localhost – Development/testing

9. COMPLIANCE CERTIFICATION
9.1 Certification Levels
BASIC: Valid .jvf structure

STANDARD: Automated verification passed

CERTIFIED: Human review + temporal anchoring

9.2 Certification Badges
Displayed in META-INF/manifest.json

Digital badge issued by Jureon Consortium

Revocable if standards violated

9.3 Certification Authorities
Jureon Consortium: Primary authority

Accredited Partners: Bar associations, courts

Self-Certification: For internal use only

10. PUBLICATION AND LICENSING
10.1 Specification License
This specification is published under:

Creative Commons CC0 1.0 Universal (public domain dedication)

Patent non-assertion pledge for implementers

10.2 Implementation Licensing
Reference implementation: Apache 2.0 License

Test suites: MIT License

Schemas: Creative Commons CC0 1.0 Universal

10.3 Trademark Policy
.jvf and Jureon are trademarks

May be used to refer to the standard

Certification marks require license

APPENDIX A: SAMPLE .JVF CREATION
Python Example:
python
import zipfile
import json
from datetime import datetime
import hashlib

def create_jvf(output_path, content):
    with zipfile.ZipFile(output_path, 'w', zipfile.ZIP_DEFLATED) as zf:
        # Write manifest
        manifest = {
            "jvfSpecVersion": "1.0.0",
            "fileUUID": f"urn:uuid:{generate_uuid()}",
            "created": datetime.utcnow().isoformat() + 'Z',
            "complianceLevel": "BASIC",
            "contentHash": calculate_hash(content)
        }
        zf.writestr('META-INF/manifest.json', 
                   json.dumps(manifest, indent=2))
        
        # Write content files
        for path, data in content.items():
            zf.writestr(path, json.dumps(data, indent=2))

# Example usage
content = {
    'CONTENT/authorities/statutes/usc-title11-507.json': {...},
    'CONTENT/logic/argument.json': {...},
    'PROOFS/verification.json': {...}
}
create_jvf('argument.jvf', content)
JavaScript Example:
javascript
const JSZip = require('jszip');

async function createJVF(content) {
    const zip = new JSZip();
    
    // Add manifest
    zip.file('META-INF/manifest.json', 
        JSON.stringify({
            jvfSpecVersion: '1.0.0',
            fileUUID: `urn:uuid:${crypto.randomUUID()}`,
            created: new Date().toISOString(),
            complianceLevel: 'BASIC'
        }, null, 2));
    
    // Add content
    Object.entries(content).forEach(([path, data]) => {
        zip.file(path, JSON.stringify(data, null, 2));
    });
    
    // Generate .jvf file
    const blob = await zip.generateAsync({
        type: 'blob',
        compression: 'DEFLATE'
    });
    return blob;
}

APPENDIX B: COMPLIANCE CHECKLIST
Basic .jvf Compliance:
Valid ZIP archive
Contains META-INF/manifest.json
All JSON files valid
Required files present
No external references
File naming conventions followed
Standard Compliance:
All authorities from primary sources
Verification steps documented
Certainty scores provided
Hash integrity verified
Schemas validate
Certified Compliance:
Temporal anchors (RFC 3161)
Digital signatures valid
Human review attested
Chain of custody complete
Revocation checks performed

APPENDIX C: FILE SIZE CONSIDERATIONS
Recommended Limits:
Maximum .jvf size: 100 MB uncompressed
Individual authority file: 10 MB maximum
Evidence files: Store externally with references
Compression: ZIP DEFLATE recommended
Large File Strategy:
Split into multiple .jvf files
Use external references with integrity hashes
Implement streaming validation
Provide summary/executive versions
END OF SPECIFICATION

Version: 1.0.0
*Date: 2024-01-15*
Status: PUBLISHED




   
