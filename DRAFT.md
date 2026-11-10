# DRAFT - ojuny-pro-template

Status: draft scope. Nothing is built. This file describes what the repository
is supposed to contain, not what it contains today.

## Purpose

The workplace edition of the Ojuny template. It targets professional and
workplace users rather than home, gaming and development users. It uses GNOME
instead of the MangoWM and Dux Modular Shell desktop, and it treats security and
a small trusted input set as first-class requirements.

The template must remain interchangeable with the regular `ojuny-template`.
The installer, the settings GUI and the patch system must be able to consume it
without a product-specific fork of their own logic.

Pro is individualized like the regular template. It is not fleet-managed and
has no enrollment path. `ojuny-fleet-controller` deploys its own integrated
`ojuny-endpoint` template to managed devices.

## Desktop

- GNOME as the only desktop. The early COSMIC edition is not part of this
  scope.
- Layout presets selected at install and changeable from the settings GUI:
  default GNOME, a Windows-like layout and a macOS-like layout.
- The preset controls panel placement, dock, overview behavior and keybindings,
  not the application set.
- GDM as the display manager, with the same account and login contract as the
  core template.
- GNOME extensions limited to a reviewed allowlist.

## Security posture

- Nixpkgs stable by default, currently targeted at the 26.11 release.
- Every flake input must come from a trusted organization. Ojuny-owned
  repositories count as trusted. Personal forks and arbitrary third-party
  flakes are not allowed by default.
- Disk encryption is offered at install and recommended by the security page.
- Secure Boot is supported through the existing post-install path, with the
  same limits as the core template.
- Firewall default-deny, SSH off by default, and no remote desktop listener
  unless explicitly enabled.
- Audit, update and integrity settings exposed as one Security page with a
  readable baseline.
- Application sources restricted to nixpkgs and an approved Flatpak remote
  where a Flatpak is required.
- No gaming stack, no launcher integrations and no Windows runtime by default.

## What the repository will contain

```text
ojuny-pro-template/
  flake.nix                  dendritic entrypoint
  flake.lock
  modules/
    01-users/                accounts and shared user settings
    02-core/                 display, input, theme, browser, terminal, updates
    03-desktop/              GNOME session pages and layout presets
    05-applications/         professional application pages
    06-utility/              management and remote-work tools
    07-system/               security, networking, hardware, boot
    backend/                 preset definitions and generated assets
    patches/                 reviewed patch switch
  scripts/                   setup, generators, lifecycle
  templates/                 settings GUI scaffolds
  docs/                      security baseline, presets, compatibility
  tests/                     evaluation and VM checks
```

### Settings pages

- Users and shared user settings.
- GNOME desktop, layout preset, displays, input, appearance.
- Browser, office, mail and calendar, VPN and remote desktop.
- Security baseline, firewall, disk encryption status, update policy.
- Networking, hardware report, boot and recovery.

### Compatibility with the core template

- Same top-level flake outputs: `nixosConfigurations.<host>`,
  `nixosModules.default`, setup apps.
- Same generated file paths: hardware, host and user modules.
- Same setup prompts and generator behavior.
- Same `@menu` and `@option` annotation rules.
- Same patch switch behavior.
- Same installer injection interface.
- A host built from this template can be consumed by the installer, the CLI
  and the lifecycle scripts without special cases. It is not consumed by Fleet.

### Documentation

- The GNOME layout presets and what each changes.
- The security baseline and each control's default.
- The trusted input policy and how to propose a new input.
- A compatibility statement against the core template.
- The differences from the home edition that an administrator should know.

### Tests

- The flake evaluates and the GNOME system builds.
- Each layout preset produces a valid session.
- The setup script produces a bootable configuration.
- The installer installs this template through the generic path.
- A no-gaming check and a no-untrusted-input check.

## Limits and non-goals

- GNOME only. No MangoWM, Dux Modular Shell or COSMIC edition.
- No fleet server and no fleet enrollment. Central management lives in
  `ojuny-fleet-controller` and `ojuny-fleet-manager`, and Fleet deploys its own
  `ojuny-endpoint` template rather than Pro.
- No general-purpose MDM or Entra ID replacement claim.
- No gaming, emulation or streaming stack.
- No secrets architecture. Pro is an individualized template.
- No custom package repository and no untrusted third-party binary cache.

## Open questions

- Is the Windows-like preset a GNOME extension set, a shell theme, or both?
- Does the security page expose a compliance report, or only configuration
  switches?
- Is Flatpak allowed at all, or is the application set nixpkgs-only?

## Sources

- `nixos-configs/notes/ojuny pro - early draft.md`.
- `ojuny-template/docs/00-overview.md`, `01-architecture.md`,
  `03-host-setup.md`, `05-settings-gui-limitations.md`.
- `ojuny-docs/08-scope-and-non-goals.md`.
