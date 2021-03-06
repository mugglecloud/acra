## 0.85.0 - 2020-12-17

- Implemented support of TLS certificate validation using OCSP and CRL (Certificate Revocation Lists)
- New configuration options were added to AcraServer and AcraConnector:
  - OCSP-related:
    - `tls_ocsp_url`, `tls_ocsp_client_url`, `tls_ocsp_database_url` - URL of OCSP server to use, for AcraServer may be configured separately for both directions
    - `tls_ocsp_required` - whether to allow "unknown" responses, whether to query all known OCSP servers (including those from certificate)
    - `tls_ocsp_from_cert` - how to treat URL listed in certificate (use or ignore, whether to prioritize over configured URL)
    - `tls_ocsp_check_only_leaf_certificate` - whether to stop validation after checking first certificate in chain (the one used for TLS handshake)
  - CRL-related:
    - `tls_crl_url`, `tls_crl_client_url`, `tls_crl_database_url` - URL of CRL distribution point to use, for AcraServer may be configured separately for both directions
    - `tls_crl_from_cert` - how to treat URL listed in certificate (use or ignore, whether to prioritize over configured URL)
    - `tls_crl_check_only_leaf_certificate` - whether to stop validation after checking first certificate in chain (the one used for TLS handshake)
    - `tls_crl_cache_size` - how many CRLs to cache in memory
    - `tls_crl_cache_time` - how long cached CRL is considered valid and won't be re-fetched

## 0.85.0 - 2020-12-08

- Extended TLS support and mapping to clientID for client's key selection purposes
  - Strategy of extraction metadata from certificates for mapping to clientID: `tls_identifier_extractor_type` (default: `distinguished_name`, another option: `serial_number`)
  - Switching to new mode with clientID extraction from certificates: `tls_client_id_from_cert`

## 0.85.0 - 2020-09-28

### Added

- More specific TLS configuration options that allow to configure separate TLS settings between client app and AcraServer, and between AcraServer and database:
  - AcraServer certificate: `tls_client_cert` and `tls_database_cert` (override `tls_cert`)
  - AcraServer key: `tls_client_key` and `tls_database_key` (override `tls_key`)
  - CA certificate path: `tls_client_ca` and `tls_database_ca` (override `tls_ca`)
  - TLS authentication: `tls_client_auth` and `tls_database_auth` (override `tls_auth`)
- Renamed database SNI option: `tls_db_sni` => `tls_database_sni`

## 0.85.0 - 2020-04-02
### Added
- Support of RHEL >= 7
- Configurable build and install parameters in Makefile (see `make help`)
- Self-documented Makefile
- `go_install.sh` script
- Makefile `pkg` target with automatic detection of OS (use it instead of `rpm` and `deb`)
### Changed
- Build image use Debian 10 instead of Debian 9
- Application names in Docker image description got `CE` suffix
- Refined logic of automatic image tagging
### Removed
- Makefile targets `dist`, `temp_copy`
- `docker_push` target replaced with `docker-push`
- default argument `--db_host=postgres` from `acra-server` docker image, specifying it explicitly is more secure
- default argument `--acraserver_connection_host=acra-server` from `acra-connector` image
### Deprecated
- `docker` target in Makefile (will be removed in 0.87.0), use `docker-build` instead
- docker images `acra-authmanager` and `acra-keymaker` (will be removed in 0.87.0); all tools are now packaged into the `acra-tools` image
- Makefile targets `rpm` and `deb` are aliases for `pkg` and will be removed in future
