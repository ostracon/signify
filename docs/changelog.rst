Release notes
=============
This page contains the most significant changes in Signify between each release.

v0.5.0 (unreleased)
-------------------
* Drop support for Python 3.6
* Add support for ECC keys
* Fix DisallowedFileTime check in Authroot parsing

v0.4.0 (2021-08-23)
-------------------
The following backwards incompatible changes were made:

* Drop support for Python 3.5
* Moved some stuff around to make more clear packages: ``signify.fingerprinter`` will remain unchanged,
  ``signify.x509`` combines certificates and their verification, ``signify.pkcs7`` combines SignedData and SignerInfo,
  and ``signify.authenticode`` contains all Microsoft-related code. This change is also reflected in how the docs
  are structured.
* Changed ``AuthenticodeSignedData.verify`` to accept ``countersignature_mode`` as an argument, replacing
  ``allow_countersignature_errors``. This allows you to skip countersignatures entirely, allowing actually using CRL
  checks (otherwise, a timestamp would be set on the context of validation, which results in certvalidator disallowing
  the CRL check because it cannot work with both timestamps and CRLs).
* Changed ``CertificateStore.verify_trust``, ``VerificationContext.verify_trust`` and
  ``CertificateTrustList.verify_trust`` to accept a certificate chain instead of a single certificate. This allows us
  to check end-entity certificates in ``CertificateTrustList``.
* ``CertificateTrustSubject.is_valid`` has been removed.

The following features were added and bugs were fixed:

* Added the functions ``explain_verify`` to ``SignedPEFile`` and ``AuthenticodeSignerInfo`` that return an
  easy-to-digest enum with the verification result.
* Added support for nested SignedData structures inside the unauthenticated attributes of SignerInfo objects. These
  are transparently added to the ``SignedPEFile.signed_datas`` iterator. You can use ``SignedPEFile.iter_signed_datas``
  to control this behaviour.
* By default, now uses a properly parsed Microsoft ``CertificateTrustList`` to allow partial removal of some
  certificates from the store, fixing a bug with our original implementation. This aligns with the implementation on
  Windows, and allows Microsoft to remove untrusted certificates from a certain timestamp, or to only allow certain
  EKU's. To restore original behaviour, use ``TRUSTED_CERTIFICATE_STORE_NO_CTL`` as certificate store.
* Fixed issue where an abnormal order in the authenticated attributes of SignerInfo objects would cause validation to
  fail.

v0.3.0 (2020-08-16)
-------------------
This release should be mostly backwards-compatible, but various features have been added that warranted a larger
version increase.

* Support for passing in a different trusted certificate store than the default in various verify functions
* Added option to ignore countersignature errors when validating
* Added support for SHA-384 and SHA-512
* Added ``Certificate.from_pems``, ``Certificate.__hash__``, ``Certificate.sha1_fingerprint``,
  ``Certificate.sha256_fingerprint``
* Added ``CertificateStore.find_certificate`` and ``CertificateStore.find_certificates``
* Added support for ``authroot.stl`` (``signify.authroot``), though we haven't figured out how it works exactly yet.
  Support can be used by adding a ctl to a trusted ``CertificateStore``.
* Updated authenticode certificate store by basing it on Microsoft's ``authroot.stl``
* Fixed bug in RFC3161 countersignatures that contain malformed RFC5652 structures
* Fixed bug in RFC3161 countersignatures that have a different digest function and hash function

v0.2.0 (2020-04-27)
-------------------
This release contains various backwards-incompatible changes.

* Fix error that SpcSpOpusInfo was considered required
* Fix error that CounterSignerInfo would require a specific content type
* Fix error that countersignatures could be present as entire RFC3161 responses
* Add option to process CRL checks and OCSP responses
* Change to use the module pyasn1-modules instead of own ASN.1 classes
* Change issuer/subject to a specific class

v0.1.5 (2019-03-16)
-------------------
* Resolve error that would cause in infinite loops in parsing of the authenticode certtable (contributed by wtfuzz)

v0.1.4 (2018-12-15)
-------------------
* Prevent iterating over duplicate certificates
* Fix bug where some samples would not be recognized as signed
* Add support for sha256 hashes
* Fix bug where countersignature verification would use the wrong digest algorithm
* Add a lot more built-in certificates
* Fix some error-handling and reporting

v0.1.3 (2018-12-15)
-------------------
* Increase minimum Python to 3.5
* Adjust location of certificate store and ensure it is included
* Add option to get a list of all potential chains
* Add option to get components of a issuer/subject

v0.1.2 (2018-03-25)
-------------------
* Change from using cryptography to using certvalidator
* Rewrite of validation routines

v0.1.1 (2018-03-25)
-------------------
* Rename to Signify
* Modify how trust is determined in a certificate store

v0.1 (2018-03-18)
-----------------
Initial release