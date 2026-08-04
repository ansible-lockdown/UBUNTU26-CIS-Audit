# Initial Ubuntu26 CIS V1.0.0 August 2026

## Alignment to CIS Ubuntu Linux 26.04 LTS Benchmark v1.0.0 - August 2026

Full alignment against v1.0.0 (350 controls). Every goss test CIS_ID and title
now matches the benchmark, and matches the remediation role control for control.

### Structure

- `section_3/cis_3.3/` split into `cis_3.3.1/` (19 IPv4 tests) and `cis_3.3.2/`
  (8 IPv6 tests); the `cis_3.3/ipv6/` subdirectory has been removed
- `section_4` reduced to `cis_4.1/` (5 UFW tests). The nftables (`cis_4.3`) and
  iptables (`cis_4.4`) suites have been removed - this benchmark covers UFW only
- `section_6/cis_6.1.1.x/` and the journald tests from `cis_6.1.2.x/` consolidated
  into `cis_6.1.1.1/` (7 tests)
- `section_6/cis_6.1.3.x/` rsyslog tests renumbered into `cis_6.1.2/` (10 tests,
  including new rsyslog-gnutls, gtls forwarding and CA certificate tests)
- `section_6/cis_6.1.4.x/` logfile access renumbered to `cis_6.1.3/`
- `section_6/cis_6.2.3/` extended from 21 to 36 auditd rule tests

### Content

- Added tests for all controls previously missing: 1.1.1.9, 1.2.1.3 - 1.2.1.15,
  1.3.1.4, 1.5.6 - 1.5.12, 1.6.4 - 1.6.12, 2.1.16, 2.4.1.7, 3.2.5 - 3.2.7,
  5.1.2, 5.1.14, 5.1.21, 5.1.22, 5.3.1.4, 6.2.3.22 - 6.2.3.36 and 7.2.11
- GDM section 1.7 reduced to the 7 v1.0.0 controls, with lock-override checks
  folded into their parent control
- Test titles aligned to benchmark wording across all 350 controls

### Fixes

- `goss.yml` referenced directories that no longer exist and gated section 4 on
  the removed `ubtu26cis_firewall_package` variable; all gossfile globs now
  resolve and every test file on disk is loaded
- `cis_6.1.2.5.yml` (was `cis_6.1.3.6.yml`) had an unquoted `*.*` that YAML
  parsed as an alias, so the file failed to load
- `cis_1.6.1.yml` used an invalid `\m` escape inside a double-quoted scalar
- `cis_3.1.1.yml` had a misindented `file:` block and a `CIS_ID` of 3.3.1
- Removed the unused `ubtu26cis_firewall_package` variable from `vars/CIS.yml`
