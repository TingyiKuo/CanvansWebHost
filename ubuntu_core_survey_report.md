# Ubuntu Core Device Evaluation Report

## Background: How Ubuntu Core Works
Unlike traditional Linux systems with a single writable root filesystem (ext4), Ubuntu Core uses a completely different approach:

**Traditional Linux:** One big root partition where everything can be modified
```
/root filesystem (ext4) - everything here, updates modify files in-place
```

**Ubuntu Core:** Multiple read-only layers stacked together
```
/snap/ubuntu-base/   <- Core OS (replaces traditional rootfs)
/snap/kernel/        <- Kernel components  
/snap/your-app/      <- Applications
```

When updates happen, Ubuntu Core doesn't modify existing files. Instead, it downloads new "snap" packages (compressed filesystems) and switches to them atomically. If something goes wrong, it instantly switches back to the previous working version.

## Executive Summary
**Bottom Line:** Ubuntu Core is a viable option for IoT/embedded devices requiring secure, automatic updates with minimal maintenance overhead.

## What is Ubuntu Core?
- Snap-based minimal Ubuntu for IoT/embedded devices
- Read-only root filesystem with atomic updates
- Automatic rollback on failed updates
- Everything packaged as snaps (containers)

## Key Advantages
- **Reliability**: Atomic updates prevent system corruption
- **Security**: Sandboxed applications with restricted system access
- **Maintenance**: Automatic updates reduce operational overhead
- **Rollback**: Failed updates automatically revert to previous working state
- **Flexibility**: Can run alongside Docker containers

## Key Considerations
- **Learning Curve**: Different from traditional Linux package management
- **Resource Overhead**: Multiple filesystem layers use more resources
- **Ecosystem**: Limited compared to traditional Ubuntu packages
- **NVIDIA Support**: Tegra platforms supported, but traditional approaches also available

## Comparison: Ubuntu Core vs Yocto + OSTree
| Feature | Ubuntu Core (Snap) | Yocto + OSTree |
|---------|-------------------|----------------|
| **Update Model** | Package-based (individual snaps) | Filesystem-based (entire rootfs) |
| **Resource Usage** | Higher (multiple layers) | Lower (single filesystem) |
| **Update Granularity** | Component-level | System-wide atomic |
| **Customization** | Limited to snap ecosystem | Full control over build |
| **Development Time** | Faster (pre-built base) | Longer (custom build system) |
| **Maintenance** | Ubuntu handles base updates | Full responsibility for all updates |
| **Rollback** | Per-snap rollback | Bootloader-based system rollback |
| **Security** | App sandboxing + confinement | Custom security policies |
| **Ecosystem** | Snap store + Ubuntu | Complete custom control |

## Update Infrastructure Options
| Option | Control Level | Cost | Complexity |
|--------|---------------|------|------------|
| Ubuntu Snap Store | Low | Free | Low |
| Brand Store | Medium | Paid | Medium |
| Self-Hosted | High | Infrastructure | High |

## Recommendation Framework
**Why Choose Ubuntu Core:**
- Device needs reliable automatic updates
- Security sandboxing is important
- Minimal maintenance overhead desired
- Planning commercial deployment with update control needs
- Faster time-to-market priority

**Why Choose Yocto + OSTree:**
- Maximum customization control required
- Resource-constrained environment
- Need complete control over system components
- Custom hardware with specific requirements

