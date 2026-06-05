---
###
# Internet-Draft Markdown Template
#
# Rename this file from draft-todo-yourname-protocol.md to get started.
# Draft name format is "draft-<yourname>-<workgroup>-<name>.md".
#
# For initial setup, you only need to edit the first block of fields.
# Only "title" needs to be changed; delete "abbrev" if your title is short.
# Any other content can be edited, but be careful not to introduce errors.
# Some fields will be set automatically during setup if they are unchanged.
#
# Don't include "-00" or "-latest" in the filename.
# Labels in the form draft-<yourname>-<workgroup>-<name>-latest are used by
# the tools to refer to the current version; see "docname" for example.
#
# This template uses kramdown-rfc: https://github.com/cabo/kramdown-rfc
# You can replace the entire file if you prefer a different format.
# Change the file extension to match the format (.xml for XML, etc...)
#
###
title: "TODO - Your title"
abbrev: "TODO - Abbreviation"
category: info
updates:
  - 5912
  - 5958

docname: draft-vangeest-update-asn1-signed-type-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: Security
workgroup: Limited Additional Mechanisms for PKIX and SMIME
keyword:
 - Public Key Infrastructure
 - ASN.1 SIGNED type
venue:
  group: Limited Additional Mechanisms for PKIX and SMIME
  type: Working Group
  mail: spasm@ietf.org
  arch: https://mailarchive.ietf.org/arch/browse/spasm/
  github: danvangeest/update-asn1-signed-type
  latest: https://example.com/LATEST

author:
 -
    inc: D. Van Geest
    fullname: Daniel Van Geest
    organization: CryptoNext Security
    email: daniel.vangeest@cryptonext-security.com

normative:
  RFC5280:
  RFC5958:

informative:
  RFC5912:

...

--- abstract

This document updates some ASN.1 modules which conform to the syntax of the 2002 version of ASN.1 but nonetheless cause errors when decoding artifacts with code compiled from these modules.
There are no bits-on-the-wire changes to any of the formats; this is simply a change to the syntax.


--- middle

# Introduction

TODO Introduction


# Conventions and Definitions

TODO Can we remove this because there is no normative terminology?
{::boilerplate bcp14-tagged}

# ASN.1 Module for [RFC5280], Explicit {#sec-5280}

SIGNED{ToBeSigned} is updated to a simpler version without ASN.1 constraints.
The SIGNATURE-ALGORITHM.&Value field is optional, and it was not valid for the SIGNED{ToBeSigned} signature CONTAINING constraint to reference a non-existent field.

~~~ asn.1
<CODE BEGINS>
PKIX1Explicit-2026
    {iso(1) identified-organization(3) dod(6) internet(1)
    security(5) mechanisms(5) pkix(7) id-mod(0)
    id-mod-pkix1-explicit-03(TBD1)}
DEFINITIONS EXPLICIT TAGS ::=
BEGIN

IMPORTS

Extensions{}, EXTENSION, ATTRIBUTE, SingleAttribute{}
FROM PKIX-CommonTypes-2009
    {iso(1) identified-organization(3) dod(6) internet(1) security(5)
    mechanisms(5) pkix(7) id-mod(0) id-mod-pkixCommon-02(57)}
AlgorithmIdentifier{}, PUBLIC-KEY, SIGNATURE-ALGORITHM
FROM AlgorithmInformation-2009
    {iso(1) identified-organization(3) dod(6) internet(1) security(5)
    mechanisms(5) pkix(7) id-mod(0)
    id-mod-algorithmInformation-02(58)}

CertExtensions, CrlExtensions, CrlEntryExtensions
FROM PKIX1Implicit-2009
    {iso(1) identified-organization(3) dod(6) internet(1) security(5)
    mechanisms(5) pkix(7) id-mod(0) id-mod-pkix1-implicit-02(59)}
SignatureAlgs, PublicKeys
FROM PKIXAlgs-2009
    {iso(1) identified-organization(3) dod(6)
    internet(1) security(5) mechanisms(5) pkix(7) id-mod(0) 56}

SignatureAlgs, PublicKeys
FROM PKIX1-PSS-OAEP-Algorithms-2009
    {iso(1) identified-organization(3) dod(6)
    internet(1) security(5) mechanisms(5) pkix(7) id-mod(0)
    id-mod-pkix1-rsa-pkalgs-02(54)}

ORAddress
FROM PKIX-X400Address-2009
    {iso(1) identified-organization(3) dod(6) internet(1) security(5)
    mechanisms(5) pkix(7) id-mod(0) id-mod-pkix1-x400address-02(60)};

id-pkix  OBJECT IDENTIFIER  ::=
    {iso(1) identified-organization(3) dod(6) internet(1) security(5)
    mechanisms(5) pkix(7)}

-- PKIX arcs

id-pe OBJECT IDENTIFIER  ::=  { id-pkix 1 }
    -- arc for private certificate extensions
id-qt OBJECT IDENTIFIER ::= { id-pkix 2 }
    -- arc for policy qualifier types
id-kp OBJECT IDENTIFIER ::= { id-pkix 3 }
    -- arc for extended key purpose OIDs
id-ad OBJECT IDENTIFIER ::= { id-pkix 48 }
    -- arc for access descriptors

-- policyQualifierIds for Internet policy qualifiers

id-qt-cps      OBJECT IDENTIFIER ::=  { id-qt 1 }
    -- OID for CPS qualifier
id-qt-unotice  OBJECT IDENTIFIER ::=  { id-qt 2 }
    -- OID for user notice qualifier
-- access descriptor definitions

id-ad-ocsp         OBJECT IDENTIFIER ::= { id-ad 1 }
id-ad-caIssuers    OBJECT IDENTIFIER ::= { id-ad 2 }
id-ad-timeStamping OBJECT IDENTIFIER ::= { id-ad 3 }
id-ad-caRepository OBJECT IDENTIFIER ::= { id-ad 5 }

-- attribute data types
AttributeType           ::=  ATTRIBUTE.&id

--  Replaced by SingleAttribute{}
--
-- AttributeTypeAndValue   ::=  SEQUENCE {
--    type    ATTRIBUTE.&id({SupportedAttributes}),
--    value   ATTRIBUTE.&Type({SupportedAttributes}{@type}) }
--

-- Suggested naming attributes: Definition of the following
--   information object set may be augmented to meet local
--   requirements.  Note that deleting members of the set may
--   prevent interoperability with conforming implementations.
-- All attributes are presented in pairs: the AttributeType
--   followed by the type definition for the corresponding
--   AttributeValue.

-- Arc for standard naming attributes

id-at OBJECT IDENTIFIER ::= { joint-iso-ccitt(2) ds(5) 4 }

-- Naming attributes of type X520name

id-at-name              AttributeType ::= { id-at 41 }
at-name ATTRIBUTE ::= { TYPE X520name IDENTIFIED BY id-at-name }

id-at-surname           AttributeType ::= { id-at 4 }
at-surname ATTRIBUTE ::= { TYPE X520name IDENTIFIED BY id-at-surname }

id-at-givenName         AttributeType ::= { id-at 42 }
at-givenName ATTRIBUTE ::=
    { TYPE X520name IDENTIFIED BY id-at-givenName }

id-at-initials          AttributeType ::= { id-at 43 }
at-initials ATTRIBUTE ::=
    { TYPE X520name IDENTIFIED BY id-at-initials }

id-at-generationQualifier AttributeType ::= { id-at 44 }
at-generationQualifier ATTRIBUTE ::=
    { TYPE X520name IDENTIFIED BY id-at-generationQualifier }
-- Directory string type --

DirectoryString{INTEGER:maxSize} ::= CHOICE {
    teletexString    TeletexString(SIZE (1..maxSize)),
    printableString  PrintableString(SIZE (1..maxSize)),
    bmpString        BMPString(SIZE (1..maxSize)),
    universalString  UniversalString(SIZE (1..maxSize)),
    uTF8String       UTF8String(SIZE (1..maxSize))
}

X520name ::= DirectoryString {ub-name}

-- Naming attributes of type X520CommonName

id-at-commonName        AttributeType ::= { id-at 3 }

at-x520CommonName ATTRIBUTE ::=
    {TYPE X520CommonName IDENTIFIED BY id-at-commonName }

X520CommonName ::= DirectoryString {ub-common-name}

-- Naming attributes of type X520LocalityName

id-at-localityName      AttributeType ::= { id-at 7 }

at-x520LocalityName ATTRIBUTE ::=
    { TYPE X520LocalityName IDENTIFIED BY id-at-localityName }
X520LocalityName ::= DirectoryString {ub-locality-name}

-- Naming attributes of type X520StateOrProvinceName

id-at-stateOrProvinceName AttributeType ::= { id-at 8 }

at-x520StateOrProvinceName ATTRIBUTE ::=
    { TYPE DirectoryString {ub-state-name}
        IDENTIFIED BY id-at-stateOrProvinceName }
X520StateOrProvinceName ::= DirectoryString {ub-state-name}

-- Naming attributes of type X520OrganizationName

id-at-organizationName  AttributeType ::= { id-at 10 }

at-x520OrganizationName ATTRIBUTE ::=
    { TYPE DirectoryString {ub-organization-name}
        IDENTIFIED BY id-at-organizationName }
X520OrganizationName ::= DirectoryString {ub-organization-name}

-- Naming attributes of type X520OrganizationalUnitName
id-at-organizationalUnitName AttributeType ::= { id-at 11 }

at-x520OrganizationalUnitName ATTRIBUTE ::=
    { TYPE DirectoryString  {ub-organizational-unit-name}
        IDENTIFIED BY id-at-organizationalUnitName }
X520OrganizationalUnitName ::= DirectoryString
                                    {ub-organizational-unit-name}

-- Naming attributes of type X520Title

id-at-title             AttributeType ::= { id-at 12 }

at-x520Title ATTRIBUTE ::= { TYPE DirectoryString { ub-title }
    IDENTIFIED BY id-at-title }

-- Naming attributes of type X520dnQualifier

id-at-dnQualifier       AttributeType ::= { id-at 46 }

at-x520dnQualifier ATTRIBUTE ::= { TYPE PrintableString
    IDENTIFIED BY id-at-dnQualifier }

-- Naming attributes of type X520countryName (digraph from IS 3166)

id-at-countryName       AttributeType ::= { id-at 6 }

at-x520countryName ATTRIBUTE ::=  { TYPE PrintableString (SIZE (2))
    IDENTIFIED BY id-at-countryName }

-- Naming attributes of type X520SerialNumber

id-at-serialNumber      AttributeType ::= { id-at 5 }

at-x520SerialNumber ATTRIBUTE ::=  {TYPE PrintableString
    (SIZE (1..ub-serial-number)) IDENTIFIED BY id-at-serialNumber }

-- Naming attributes of type X520Pseudonym

id-at-pseudonym         AttributeType ::= { id-at 65 }

at-x520Pseudonym ATTRIBUTE ::= { TYPE DirectoryString {ub-pseudonym}
    IDENTIFIED BY id-at-pseudonym }

-- Naming attributes of type DomainComponent (from RFC 2247)

id-domainComponent      AttributeType ::=
    { itu-t(0) data(9) pss(2342) ucl(19200300) pilot(100)
    pilotAttributeType(1) 25 }
at-domainComponent ATTRIBUTE ::= {TYPE IA5String
    IDENTIFIED BY id-domainComponent }

-- Legacy attributes

pkcs-9 OBJECT IDENTIFIER ::=
    { iso(1) member-body(2) us(840) rsadsi(113549) pkcs(1) 9 }
id-emailAddress          AttributeType ::= { pkcs-9 1 }

at-emailAddress ATTRIBUTE ::= {TYPE IA5String
    (SIZE (1..ub-emailaddress-length)) IDENTIFIED BY
    id-emailAddress }

-- naming data types --

Name ::= CHOICE { -- only one possibility for now --
    rdnSequence  RDNSequence }

RDNSequence ::= SEQUENCE OF RelativeDistinguishedName

DistinguishedName ::=   RDNSequence

RelativeDistinguishedName  ::=
    SET SIZE (1 .. MAX) OF SingleAttribute { {SupportedAttributes} }

--  These are the known name elements for a DN

SupportedAttributes ATTRIBUTE ::= {
    at-name | at-surname | at-givenName | at-initials |
    at-generationQualifier | at-x520CommonName |
    at-x520LocalityName | at-x520StateOrProvinceName |
    at-x520OrganizationName | at-x520OrganizationalUnitName |
    at-x520Title | at-x520dnQualifier | at-x520countryName |
    at-x520SerialNumber | at-x520Pseudonym | at-domainComponent |
    at-emailAddress, ... }

--
-- Certificate- and CRL-specific structures begin here
--

Certificate  ::=  SIGNED{TBSCertificate}

TBSCertificate  ::=  SEQUENCE  {
    version         [0]  Version DEFAULT v1,
    serialNumber         CertificateSerialNumber,
    signature            AlgorithmIdentifier{SIGNATURE-ALGORITHM,
                            {SignatureAlgorithms}},
    issuer               Name,
    validity             Validity,
    subject              Name,
    subjectPublicKeyInfo SubjectPublicKeyInfo,
    ... ,
    [[2:               -- If present, version MUST be v2
    issuerUniqueID  [1]  IMPLICIT UniqueIdentifier OPTIONAL,
    subjectUniqueID [2]  IMPLICIT UniqueIdentifier OPTIONAL
    ]],
    [[3:               -- If present, version MUST be v3 --
    extensions      [3]  Extensions{{CertExtensions}} OPTIONAL
    ]], ... }

Version  ::=  INTEGER  {  v1(0), v2(1), v3(2)  }

CertificateSerialNumber  ::=  INTEGER

Validity ::= SEQUENCE {
    notBefore      Time,
    notAfter       Time  }

Time ::= CHOICE {
    utcTime        UTCTime,
    generalTime    GeneralizedTime }

UniqueIdentifier  ::=  BIT STRING

SubjectPublicKeyInfo  ::=  SEQUENCE  {
    algorithm            AlgorithmIdentifier{PUBLIC-KEY,
                            {PublicKeyAlgorithms}},
    subjectPublicKey     BIT STRING  }

-- CRL structures

CertificateList  ::=  SIGNED{TBSCertList}

TBSCertList  ::=  SEQUENCE  {
    version              Version OPTIONAL,
                                -- if present, MUST be v2
    signature            AlgorithmIdentifier{SIGNATURE-ALGORITHM,
                            {SignatureAlgorithms}},
    issuer               Name,
    thisUpdate           Time,
    nextUpdate           Time OPTIONAL,
    revokedCertificates  SEQUENCE SIZE (1..MAX) OF SEQUENCE {
        userCertificate  CertificateSerialNumber,
        revocationDate   Time,
        ... ,
        [[2:                  -- if present, version MUST be v2
        crlEntryExtensions  Extensions{{CrlEntryExtensions}}
                                OPTIONAL
        ]], ...
    } OPTIONAL,
    ... ,
    [[2:                       -- if present, version MUST be v2
    crlExtensions       [0] Extensions{{CrlExtensions}}
                                OPTIONAL
    ]], ... }

-- Version, Time, CertificateSerialNumber, and Extensions were
-- defined earlier for use in the certificate structure

--
--  The two object sets below should be expanded to include
--  those algorithms which are supported by the system.
--
--  For example:
--  SignatureAlgorithms SIGNATURE-ALGORITHM ::= {
--    PKIXAlgs-2008.SignatureAlgs, ...,
--        - - RFC 3279 provides the base set
--    PKIX1-PSS-OAEP-ALGORITHMS.SignatureAlgs |
--        - - RFC 4055 provides extension algs
--    OtherModule.SignatureAlgs
--        - - RFC XXXX provides additional extension algs
--  }

SignatureAlgorithms SIGNATURE-ALGORITHM ::= {
    PKIXAlgs-2009.SignatureAlgs, ...,
    PKIX1-PSS-OAEP-Algorithms-2009.SignatureAlgs }

PublicKeyAlgorithms PUBLIC-KEY ::= {
    PKIXAlgs-2009.PublicKeys, ...,
    PKIX1-PSS-OAEP-Algorithms-2009.PublicKeys}

-- Upper Bounds

ub-state-name INTEGER ::= 128
ub-organization-name INTEGER ::= 64
ub-organizational-unit-name INTEGER ::= 64
ub-title INTEGER ::= 64
ub-serial-number INTEGER ::= 64
ub-pseudonym INTEGER ::= 128
ub-emailaddress-length INTEGER ::= 255
ub-locality-name INTEGER ::= 128
ub-common-name INTEGER ::= 64
ub-name INTEGER ::= 32768
-- Note - upper bounds on string types, such as TeletexString, are
-- measured in characters.  Excepting PrintableString or IA5String, a
-- significantly greater number of octets will be required to hold
-- such a value.  As a minimum, 16 octets or twice the specified
-- upper bound, whichever is the larger, should be allowed for
-- TeletexString.  For UTF8String or UniversalString, at least four
-- times the upper bound should be allowed.

-- Information object classes used in the definition
-- of certificates and CRLs

-- Parameterized Type SIGNED
--
-- Three different versions of doing SIGNED:
--  1.  Simple and close to the PKIX1Explicit93 (RFC 2459) version
--
SIGNED{ToBeSigned} ::= SEQUENCE {
    toBeSigned          ToBeSigned,
    algorithmIdentifier AlgorithmIdentifier{SIGNATURE-ALGORITHM,
                        {SignatureAlgorithms}},
    signature           BIT STRING
}

--  2.  From Authenticated Framework
--
--  SIGNED{ToBeSigned} ::= SEQUENCE {
--    toBeSigned        ToBeSigned,
--    COMPONENTS OF SIGNATURE{ToBeSigned}
--  }
--  SIGNATURE{ToBeSigned} ::= SEQUENCE {
--    algorithmIdentifier   AlgorithmIdentifier,
--    encrypted             ENCRYPTED-HASH{ToBeSigned}
--  }
--  ENCRYPTED-HASH{ToBeSigned} ::=
--    BIT STRING
--      (CONSTRAINED BY {
--        shall be the result of applying a hashing procedure to
--        the DER-encoded (see 4.1) octets of a value of
--        ToBeSigned and then applying an encipherment procedure
--        to those octets
--      })
--
--
--  3.  Removed since PKIX1Explicit-2009:
--      A more complex version, but one that automatically ties
--      together both the signature algorithm and the
--      signature value for automatic decoding.

END
<CODE ENDS>
~~~

# ASN.1 Module for [RFC5958] {#sec-5958}

The only change for this module from the AsymmetricKeyPackageModuleV1 module in [RFC5958] is to remove the commented-out alternative representation of OneAsymmetricKey which made full use of ASN.1 constraints.
The PUBLIC-KEY.&PrivateKey field is optional, and it was not valid for the OneAsymmetricKey privateKey CONTAINING constraint to reference a non-existent field.
For an ASN.1 compiler, there is no difference between the AsymmetricKeyPackageModuleV1 and AsymmetricKeyPackageModuleV1-2026 modules.

~~~ asn.1
<CODE BEGINS>
AsymmetricKeyPackageModuleV1-2026
  { iso(1) member-body(2) us(840) rsadsi(113549) pkcs(1) pkcs-9(9)
    smime(16) modules(0) id-mod-asymmetricKeyPkgV1-2026(TBD2) }

DEFINITIONS IMPLICIT TAGS ::=

BEGIN

-- EXPORTS ALL

IMPORTS

-- FROM New SMIME ASN.1 [RFC5911]

Attribute{}, CONTENT-TYPE
FROM CryptographicMessageSyntax-2009
  { iso(1) member-body(2) us(840) rsadsi(113549) pkcs(1) pkcs-9(9)
    smime(16) modules(0) id-mod-cms-2004-02(41) }

-- From New PKIX ASN.1 [RFC5912]
ATTRIBUTE
FROM PKIX-CommonTypes-2009
  { iso(1) identified-organization(3) dod(6) internet(1)
    security(5) mechanisms(5) pkix(7) id-mod(0)
    id-mod-pkixCommon-02(57) }

-- From New PKIX ASN.1 [RFC5912]

AlgorithmIdentifier{}, ALGORITHM, PUBLIC-KEY, CONTENT-ENCRYPTION
  FROM AlgorithmInformation-2009
    { iso(1) identified-organization(3) dod(6) internet(1)
      security(5) mechanisms(5) pkix(7) id-mod(0)
      id-mod-algorithmInformation-02(58) }

;

ContentSet CONTENT-TYPE ::= {
ct-asymmetric-key-package,
... -- Expect additional content types --
}
ct-asymmetric-key-package CONTENT-TYPE ::=
{ AsymmetricKeyPackage IDENTIFIED BY id-ct-KP-aKeyPackage }

id-ct-KP-aKeyPackage OBJECT IDENTIFIER ::=
  { joint-iso-itu-t(2) country(16) us(840) organization(1)
      gov(101) dod(2) infosec(1) formats(2)
      key-package-content-types(78) 5
  }

AsymmetricKeyPackage ::= SEQUENCE SIZE (1..MAX) OF OneAsymmetricKey

OneAsymmetricKey ::= SEQUENCE {
  version                   Version,
  privateKeyAlgorithm       PrivateKeyAlgorithmIdentifier,
  privateKey                PrivateKey,
  attributes            [0] Attributes OPTIONAL,
  ...,
  [[2: publicKey        [1] PublicKey OPTIONAL ]],
  ...
}

PrivateKeyInfo ::= OneAsymmetricKey

-- PrivateKeyInfo is used by [P12]. If any items tagged as version
-- 2 are used, the version must be v2, else the version should be
-- v1. When v1, PrivateKeyInfo is the same as it was in [RFC5208].

Version ::= INTEGER { v1(0), v2(1) } (v1, ..., v2)

PrivateKeyAlgorithmIdentifier ::= AlgorithmIdentifier
                                  { PUBLIC-KEY,
                                    { PrivateKeyAlgorithms } }

PrivateKey ::= OCTET STRING
                  -- Content varies based on type of key. The
                  -- algorithm identifier dictates the format of
                  -- the key.

PublicKey ::= BIT STRING
                  -- Content varies based on type of key. The
                  -- algorithm identifier dictates the format of
                  -- the key.

Attributes ::= SET OF Attribute { { OneAsymmetricKeyAttributes } }

OneAsymmetricKeyAttributes ATTRIBUTE ::= {
  ... -- For local profiles
}

EncryptedPrivateKeyInfo ::= SEQUENCE {
  encryptionAlgorithm  EncryptionAlgorithmIdentifier,
  encryptedData        EncryptedData }

EncryptionAlgorithmIdentifier ::= AlgorithmIdentifier
                                    { CONTENT-ENCRYPTION,
                                      { KeyEncryptionAlgorithms } }

EncryptedData ::= OCTET STRING -- Encrypted PrivateKeyInfo

PrivateKeyAlgorithms ALGORITHM ::= {
  ... -- Extensible
}

KeyEncryptionAlgorithms ALGORITHM ::= {
  ... -- Extensible
}

END
<CODE ENDS>
~~~


# Security Considerations

Even though all the RFCs in this document are security-related, the document itself does not have any security considerations.
The ASN.1 modules keep the same bits-on-the-wire as the modules that they replace.

# IANA Considerations

IANA is requested to allocate a value from the "SMI Security for PKIX Module Identifier (1.3.6.1.5.5.7.0)" registry for the ASN.1 module in {{sec-5280}}.

*  Decimal: IANA Assigned - *Replace TBD1*

*  Description: PKIX1Explicit-2026 - id-mod-pkix1-explicit-03

IANA is requested to allocate a value from the "SMI Security for S/MIME Module Identifier (1.2.840.113549.1.9.16.0)" registry for the ASN.1 module in {{sec-5958}}.

*  Decimal: IANA Assigned - *Replace TBD2*

*  Description: AsymmetricKeyPackageModuleV1-2026 - id-mod-asymmetricKeyPkgV1-2026


--- back

# Acknowledgments
{:numbered="false"}

Thanks goes to Pierce Leonberger who submitted an errata for SIGNED{ToBeSigned} in 2014 which didn't get the attention it deserved.
Thanks to Josef Frühwirth who brought up the issue again more recently.
