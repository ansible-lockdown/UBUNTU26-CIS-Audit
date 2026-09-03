# Initial Ubuntu26 CIS V1.0.0 August 2026

## September 2026 - contributing guidelines

- CONTRIBUTING.md replaced with the canonical org-wide copy. It now states that pull
  requests come from approved contributors, how to be onboarded through the Discord
  server, and that issues remain open to everyone
- README gained a Contributing section under Support pointing at CONTRIBUTING.md

## September 2026 - three-way alignment to v1.0.0

- 5.4.1.2 carried a second test whose title and CIS_ID were copy-pasted from 5.4.1.1.
  The test itself is correct for 5.4.1.2 - it reads field 4 of /etc/shadow, the minimum
  days - but it was labelled 5.4.1.1 and carried 5.4.1.1's Level 1 meta while sitting
  inside the level_2 file gate. Relabelled to 5.4.1.2 with server/workstation 2, which
  is what the benchmark's Profile Applicability states
- 2.2.1, 2.2.5, 5.1.13 and 5.4.2.5 titles aligned to the benchmark wording
- separator spacing normalised to " | " in 5.3.3.3.3 and 6.2.3.18-6.2.3.20

## September 2026 - tests that were never evaluated

- 1.6.6 declared `exec:` as the top-level resource type. That is not a goss resource -
  the type is `command:` with `exec:` as its attribute - so the /etc/motd permission
  check was never evaluated
- 2.4.1.3-2.4.1.8 shared one file with the `file:` resource declared inside the
  `rule_2_4_1_3` gate. Turning that one rule off while any of 2.4.1.4-.8 stayed on
  emitted child keys with no parent resource type. Split into one file per control,
  matching the Ubuntu24 layout; the `section_2/cis_2.4/*.yml` glob picks them up
- 1.1.1.6 asserted `install overlayfs` and `blacklist overlayfs` while the remediation
  correctly writes `overlay` - the module has not been called overlayfs since the 3.x
  kernels - so the audit failed a correctly remediated host

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

## Fixes from testing on Ubuntu 26.04 hardware - August 2026

Found by running `run_audit.sh` standalone, and by the role-driven audit, against
a real Ubuntu 26.04 host. The standalone path exercises `vars/CIS.yml`, which the
role-driven path never uses - it renders its own vars from the remediation
defaults - so these were invisible until the audit was run on its own.

- **`ubtu26cis_time_pool` was missing its `options` key.** `cis_2.3.3.1.yml`
  iterates the list and reads `{{ .options }}`; the missing key aborted goss at
  load, so **all 874 tests produced no output at all**. `defaults/main.yml` had
  `options: iburst maxsources 4` and `vars/CIS.yml` did not - a nested
  list-of-dicts mismatch that scalar config-parity checks do not detect.
- **`cis_6.2.4.8/9/10` no longer stat absent audit tools.** They ran
  `stat` over a fixed list including `/sbin/autrace`, which Ubuntu 26.04 does not
  ship; `stat` then exits non-zero and the control fails permanently. Each now
  stats only the tools present.
- **`ubtu26cis_sysctl_network_conf` updated** to `/etc/sysctl.d/97_cis_network.conf`
  to match the remediation role. The 3.3.x tests already grep both
  `/etc/sysctl.conf` and `/etc/sysctl.d/*`, so no test change was needed.

### Verified result

Standalone `run_audit.sh` on Ubuntu 26.04: 868 tests execute cleanly.
Role-driven pre/post: 369 -> 39 failures.
