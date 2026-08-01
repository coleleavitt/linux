```markdown
# linux Development Patterns

> Auto-generated skill from repository analysis

## Overview
This skill teaches you the key development patterns, coding conventions, and common workflows used in the Rust portions of the `linux` repository. It covers how to contribute fixes, improvements, and backports to various drivers and subsystems, with a focus on clear commit messages, file organization, and best practices for code changes. Whether you're fixing a bug in a single driver, applying a coordinated patch series, or backporting upstream changes, this guide provides step-by-step instructions and code examples to help you follow the repository's standards.

## Coding Conventions

- **File Naming:**  
  Use `snake_case` for file names.
  ```
  // Good
  mod my_driver.rs
  // Bad
  mod MyDriver.rs
  ```

- **Import Style:**  
  Use relative imports.
  ```rust
  // Good
  use super::helper;
  use crate::utils::math;
  // Bad
  use linux::utils::math;
  ```

- **Export Style:**  
  Use named exports.
  ```rust
  // Good
  pub fn initialize() { ... }
  pub struct Device { ... }
  // Bad
  pub mod device { ... }
  ```

- **Commit Messages:**  
  - Use freeform messages, often prefixed with the subsystem or driver name (e.g., `ntfs:`, `drivers:`, `mshv:`).
  - Average commit message length is about 59 characters.
  - Include detailed explanations, referencing bugs, fixes, or cherry-picked commits when relevant.

## Workflows

### driver-bugfix-single-file
**Trigger:** When a bug is found in a driver and needs to be fixed.  
**Command:** `/fix-driver-bug`

1. Identify the bug and its root cause in the driver source file.
2. Edit the relevant driver source file to fix the bug.
3. Write a detailed commit message explaining the bug, how it was fixed, and reference any relevant bug reports or Fixes tags.
4. Commit the change.

**Example:**
```rust
// Before: Potential null pointer dereference
if ptr.is_some() {
    ptr.unwrap().do_work();
}

// After: Safe dereference
if let Some(val) = ptr {
    val.do_work();
}
```
Commit message:
```
drivers/usb: Fix null pointer dereference in usb_probe()

Fixes a crash when device pointer is unexpectedly null. Ensured safe
dereferencing using pattern matching.
```

---

### drm-vmwgfx-multi-file-bugfix-series
**Trigger:** When a set of related bugs or refactorings are needed in the drm/vmwgfx driver.  
**Command:** `/drm-vmwgfx-bugfix-series`

1. Identify a set of related issues or improvements in drm/vmwgfx.
2. For each issue, edit the relevant file (e.g., validation, blit, execbuf, etc.) to address the problem.
3. Write a detailed commit message for each fix, referencing the specific bug and file.
4. Commit each change separately, often as a patch series.

**Example:**
```rust
// Fix in vmwgfx_blit.c
fn blit_operation() {
    // ...fix bug in blit logic...
}
```
Commit message:
```
drm/vmwgfx: Fix blit operation overflow

Corrects integer overflow in blit size calculation.
```

---

### amdgpu-amdkfd-fix-and-backport
**Trigger:** When a bug is found in AMDGPU/AMDKFD or a fix needs to be backported.  
**Command:** `/amdgpu-fix`

1. Identify the bug or improvement in AMDGPU/AMDKFD.
2. Edit the relevant AMDGPU/AMDKFD source file(s) to fix the issue.
3. Include a 'Fixes' or 'cherry picked from' reference in the commit message.
4. Commit the change.

**Example:**
```rust
// Patch for amdgpu_device.c
fn handle_error() {
    // ...improved error handling...
}
```
Commit message:
```
drm/amdgpu: Fix error handling in device init

Fixes: 1234abcd ("drm/amdgpu: Initial device support")
(cherry picked from commit 5678efgh)
```

---

### mediatek-drm-i2c-adapter-leak-fix
**Trigger:** When an I2C adapter leak or improper reference handling is detected in Mediatek DRM HDMI code.  
**Command:** `/mediatek-i2c-leak-fix`

1. Identify the I2C adapter leak or improper reference handling.
2. Switch to `of_get_i2c_adapter_by_node()` for adapter acquisition.
3. Ensure references are released in the correct code paths (e.g., `.destroy` handler).
4. Commit the fix with a detailed message.

**Example:**
```rust
// Before
let adapter = i2c_get_adapter(id);

// After
let adapter = of_get_i2c_adapter_by_node(node);
```
Commit message:
```
drm/mediatek: Fix I2C adapter reference leak in HDMI

Switch to of_get_i2c_adapter_by_node() and ensure proper release in destroy handler.
```

---

### ntfs-bugfix-single-file
**Trigger:** When a bug is found in NTFS driver code.  
**Command:** `/ntfs-fix`

1. Identify the NTFS bug and its cause.
2. Edit the relevant NTFS source file to fix the bug.
3. Write a detailed commit message referencing the bug and the fix.
4. Commit the change.

**Example:**
```rust
// Before: Incorrect bounds check
if index > len {
    // ...
}

// After: Correct bounds check
if index >= len {
    // ...
}
```
Commit message:
```
ntfs: Fix out-of-bounds access in attribute parser

Ensures index is strictly less than length.
```

---

### hyperv-mshv-bugfix
**Trigger:** When a bug is found in Hyper-V or mshv code.  
**Command:** `/mshv-fix`

1. Identify the bug in Hyper-V/mshv code.
2. Edit the relevant source file to fix the bug (e.g., struct definition, fd leak, memory clearing).
3. Write a commit message referencing the bug and fix.
4. Commit the change.

**Example:**
```rust
// Before: Missing memory clear
let mut data = SomeStruct::default();

// After: Explicit memory clear
let mut data = SomeStruct::default();
memset(&mut data, 0, size_of::<SomeStruct>());
```
Commit message:
```
mshv: Zero memory before use in ioctl handler

Prevents use of uninitialized memory.
```

---

## Testing Patterns

- **Test Framework:** Unknown (not explicitly detected).
- **Test File Pattern:** Test files are named with `*.test.*`.
- **Typical Usage:**  
  - Tests are likely placed alongside source files or in dedicated test directories.
  - To run tests, look for files matching the `*.test.*` pattern and use the appropriate Rust test runner or custom scripts.

**Example:**
```rust
// In foo.test.rs
#[test]
fn test_driver_initialization() {
    // ...test logic...
}
```

## Commands

| Command                  | Purpose                                                         |
|--------------------------|-----------------------------------------------------------------|
| /fix-driver-bug          | Fix a bug in a single driver source file                        |
| /drm-vmwgfx-bugfix-series| Apply a coordinated bugfix/refactor series to drm/vmwgfx files  |
| /amdgpu-fix              | Fix or backport a change in AMDGPU/AMDKFD drivers               |
| /mediatek-i2c-leak-fix   | Fix I2C adapter reference leaks in Mediatek DRM HDMI code       |
| /ntfs-fix                | Fix a bug in the NTFS driver                                    |
| /mshv-fix                | Fix a bug in Hyper-V or mshv code                               |
```
