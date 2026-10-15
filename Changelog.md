# Initial Ubuntu26 CIS V1.0.0 August 2026

## CIS Benchmark 1.0.0

## October 2026 - QA fixes

- run_audit.sh uses syver v0.15.1 by default
- README describes syver
- README audit options and link block corrected
- LICENSE: company name updated to MindPoint Group - A Quantum Sky Company
- meta levels and level gates aligned to Profile Applicability
- 5.1.4 public host key perms test fixed: variable name, mask /133
- 5.1.4 find -name globs quoted
- 6.1.1.1.3 checks the effective ForwardToSyslog value is yes
- 6.1.1.1.5 to 6.1.1.1.7 journald tests read systemd-analyze cat-config
- 7.2.5 to 7.2.8 duplicate checks sort before uniq -d
- 6.2.4.4 accepts any audit log directory mode without group write or other access
- 5.1.6 sshd_config.d banner check reads the drop-in files
- 5.2.3 checks journal sudo events or the sudo logfile setting
- 5.4.2.8 uses the benchmark valid shell check
- sysctl config tests no longer fail when /etc/sysctl.conf is absent
- 2.2.1 gated on ubtu26cis_nis_client_required
- 2.1.3 checks the kea packages and units
- 1.2.1.3, 1.2.1.6 and 1.2.1.9 mode patterns accept stat output without a leading zero
- 1.2.1.10 passes when sources.list has no http entries
- 1.2.1.11 checks only .list and .sources files
- 1.6.9 checks the motd files named in /etc/pam.d
- 4.1.5 accepts disabled (routed) with a DROP or REJECT forward policy
- 5.4.2.4 accepts a locked root account
- 5.1.14 requires both post-quantum key exchange algorithms
- 1.7.1 to 1.7.5 test the dconf files the role writes, section headers matched literally
- 1.7.3 to 1.7.5 lock file names and patterns corrected, autorun-never typo fixed
- 5.1.3 find -name globs quoted
- 5.2.2 sudoers check reports any !use_pty override
- 5.4.1.5 per-user INACTIVE check uses the benchmark condition
- 5.4.2.6 checks /root/.bashrc and /root/.profile
- 6.2.3.35 matches the -e 2 line explicitly
- 6.2.3.x expect the benchmark path and dir rules instead of -w watches
- 6.2.3.4 checks the clock_settime a0 filter and the localtime-change rule
- 6.2.3.8 and 6.2.3.9 accept an absent interfaces.d or NetworkManager directory
- 6.2.4.3 flags log files not owned by group root or adm
- 1.6.12 meta workstation set to NA
- 6.3.2 defaults to the timer; service may be static or enabled, cron check requires an aide job
- 3.1.2 wireless check quoted
- 5.2.7 su group matched by name
- 6.1.2.4 expects auth logging in /var/log/auth.log
- 6.2.3.18, 6.2.3.25, 6.2.3.32 and 6.2.3.33 match the benchmark syscall rules
- 6.2.4.8 to 6.2.4.10 and 6.3.3 no longer list autrace
- unused ubtu26cis_ipv4_required and ubtu26cis_legacy_boot removed

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
