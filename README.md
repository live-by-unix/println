# PrintLN - Bash CLI Print Tool

A lightweight, production-ready Bash CLI tool for printing documents to network and local printers with an interactive workflow and non-interactive mode. No Node.js dependencies required!

  <img width="1264" height="1264" alt="printlnlogo" src="https://github.com/user-attachments/assets/8549e47a-0d4f-4db1-82f7-a6755e6ba36a" />

## Features

- 🖨️ **System Printer Integration** - Automatically detects available printers via CUPS
- 📄 **Multiple File Formats** - Supports PDF, TXT, MD, Markdown, CSV, DOCX, PNG, and JPEG
- 🔄 **Automatic Document Conversion** - Converts DOCX to PDF and Markdown to PDF automatically
- 🎨 **Color Mode Selection** - Choose between Black & White and Color printing
- 📑 **Advanced Print Options** - Duplex modes, collation, paper size selection
- ✅ **Interactive & Non-Interactive Modes** - Choose your workflow
- 🚀 **Lightweight** - Pure Bash, no external dependencies (beyond CUPS)
- 💻 **Cross-Platform** - Works on macOS, Linux, and Unix systems
- 🔧 **Broad Bash Support** - Compatible with Bash 3.2+ (including older systems)

## Requirements

### Core Requirements
- Bash 3.2+ (tested on 3.2, 4.0+, and 5.0+)
- CUPS (Common Unix Printing System) installed and configured
- `lp` command-line utility (comes with CUPS)
- `lpstat` utility for printer detection

### Optional Requirements (for file conversion)
- **For DOCX support**: LibreOffice or OpenOffice
  ```bash
  # macOS
  brew install libreoffice
  
  # Ubuntu/Debian
  sudo apt-get install libreoffice
  
  # RHEL/CentOS
  sudo yum install libreoffice
  ```

- **For Markdown support**: Pandoc (recommended) or Markdown
  ```bash
  # macOS
  brew install pandoc
  
  # Ubuntu/Debian
  sudo apt-get install pandoc
  
  # RHEL/CentOS
  sudo yum install pandoc
  ```

## Installation 

```bash
# Clone the repository
git clone https://github.com/live-by-unix/println.git
cd println

# Make the script executable
chmod +x println

# Optional: Install globally
sudo cp println /usr/local/bin/println
```

If you want to use it as a command but don't have sudo, add this to your `~/.bashrc` or `~/.zshrc`:

```bash
alias println="/path/to/your/println"
```

Then reload your shell:
```bash
source ~/.bashrc  # or source ~/.zshrc
```

### Verify Installation

```bash
# Check if println is accessible
println -v

# Output:
# println v3.1.0
```

## Usage

### Basic Commands

Show version:
```bash
println -v
# or
println --version
```

Show help:
```bash
println -h
# or
println help
# or
println --help
```

Start interactive print workflow:
```bash
println print
```

Start non-interactive print job:
```bash
println print --no-interactive [OPTIONS]
```

## Interactive Workflow

When you run `println print`, the tool guides you through these steps:

### Step 1: Select Printer
```
ℹ️  Scanning for available printers...

  1. Brother_HL_L8360CDW
  2. HP_LaserJet_Pro_M404n
  3. Canon_imagERUNNER_2530

Select a printer (enter number): 1

✅ Selected printer: Brother_HL_L8360CDW
```

### Step 2: Enter File Path
```
Enter absolute path of file to print: /Users/john/documents/report.pdf

✅ File validated: report.pdf
```

### Step 3: Select Color Mode
```
Select color mode:
  1. Black & White
  2. Color

Enter choice (1 or 2): 1

✅ Selected color mode: Black & White
```

### Step 4: Select Duplex Mode
```
Select duplex mode:
  1. Single-sided
  2. Double-sided (long-edge)
  3. Double-sided (short-edge)

Enter choice (1, 2, or 3): 2

✅ Selected duplex mode: Double-sided (long-edge)
```

### Step 5: Select Paper Size
```
Select paper size:
  1. Letter
  2. Legal
  3. A4
  4. A3
  5. A5
  6. Tabloid

Enter choice (1-6): 3

✅ Selected paper size: A4
```

### Step 6: Select Collate Option
```
Collate pages?
  1. Yes
  2. No

Enter choice (1 or 2): 1

✅ Selected collate: Yes
```

### Step 7: Enter Number of Copies
```
Enter number of copies: 2

✅ Selected copies: 2
```

### Step 8: Review Summary
```
📋 Print Job Summary:
──────────────────────────────────────────────
Printer:       Brother_HL_L8360CDW
File Path:     /Users/john/documents/report.pdf
File Name:     report.pdf
File Size:     245.32 KB
Color Mode:    Black & White
Duplex Mode:   Double-sided (long-edge)
Paper Size:    A4
Collate:       Yes
Copies:        2
──────────────────────────────────────────────
```

### Step 9: Confirm & Submit
```
Proceed with print job? (y/N): y

ℹ️  Submitting print job...

✅ Print job submitted successfully!
📌 Printer: Brother_HL_L8360CDW
📄 File: report.pdf
📋 Copies: 2
🔄 Duplex: Double-sided (long-edge)
📑 Collate: Yes
```

## Non-Interactive Mode

Non-interactive mode allows you to specify all print options directly via command-line arguments, perfect for automation, scripts, and CI/CD pipelines.

### Syntax

```bash
println print --no-interactive [OPTIONS]
```

### Required Options

- `-p, --printer <name>` - Printer name (must be available on system)
- `-f, --file <path>` - Absolute file path to print

### Optional Options

- `-c, --color <mode>` - Color mode: `bw` (default) or `color`
- `-d, --duplex <mode>` - Duplex mode: `one-sided`, `long-edge` (default), or `short-edge`
- `-n, --copies <number>` - Number of copies (default: 1)
- `-s, --paper-size <number>` - Paper size number (default: 3 for A4):
  - `1` = Letter
  - `2` = Legal
  - `3` = A4
  - `4` = A3
  - `5` = A5
  - `6` = Tabloid
- `--collate <yes|no>` - Collate pages (default: yes)
- `--both-sides <yes|no>` - Print on both sides (default: yes)

### Non-Interactive Examples

**Example 1: Simple Color Print (PDF)**
```bash
println print --no-interactive -p "Office_Printer" -f "/home/user/report.pdf" -c color
```

**Example 2: Print DOCX Document (auto-converts to PDF)**
```bash
println print --no-interactive -p "HP_LaserJet" -f "/home/user/document.docx" -n 2 -s 2
# Automatically converts DOCX to PDF, prints 2 copies on Legal-sized paper
```

**Example 3: Print Markdown File (auto-converts to PDF)**
```bash
println print --no-interactive -p "Office_Printer" -f "/home/user/README.md" -c color
# Automatically converts Markdown to PDF and prints in color
```

**Example 4: Multi-Copy with Custom Paper Size**
```bash
println print --no-interactive -p "HP_LaserJet" -f "/home/user/document.pdf" -n 3 -s 2
# Prints 3 copies on Legal-sized paper
```

**Example 5: No Collation, Single-Sided**
```bash
println print --no-interactive -p "Brother" -f "/home/user/file.pdf" --collate no --both-sides no
```

**Example 6: Double-Sided Short-Edge with Custom Settings**
```bash
println print --no-interactive -p "Canon" -f "/home/user/image.png" -d short-edge -n 2 -c color --collate yes
```

**Example 7: A3 Paper, Color, 5 Copies**
```bash
println print --no-interactive -p "Office_Printer" -f "/home/user/poster.pdf" -s 4 -c color -n 5
```

**Example 8: Automation Script - Batch Print Multiple Files**
```bash
#!/bin/bash
for file in /path/to/documents/*.pdf; do
  println print --no-interactive \
    -p "Office_Printer" \
    -f "$file" \
    -c bw \
    -n 1 \
    --collate yes \
    --both-sides yes
done
```

**Example 9: Batch Print DOCX Files (auto-convert)**
```bash
#!/bin/bash
for file in /path/to/reports/*.docx; do
  println print --no-interactive \
    -p "Office_Printer" \
    -f "$file" \
    -c bw \
    -n 1 \
    -d long-edge
done
```

## Supported File Types

### Direct Printing
- **Documents**: PDF, TXT, CSV
- **Images**: PNG, JPEG

### Auto-Conversion to PDF
- **Office Documents**: DOCX (Word) - requires LibreOffice
- **Markdown**: MD, Markdown - requires Pandoc

## Supported Paper Sizes

- Letter (8.5" × 11")
- Legal (8.5" × 14")
- A4 (210mm × 297mm) - Default
- A3 (297mm × 420mm)
- A5 (148mm × 210mm)
- Tabloid (11" × 17")

## Supported Duplex Modes

- **Single-sided** - Prints on one side only
- **Double-sided (long-edge)** - Default binding on long edge
- **Double-sided (short-edge)** - Binding on short edge

## Troubleshooting

### "No printers found on the system"
**Solution:**
- Ensure CUPS is installed: `brew install cups` (macOS) or `sudo apt-get install cups` (Linux)
- Start CUPS service: `sudo systemctl start cups` (Linux) or `sudo launchctl start org.cups.cupsd` (macOS)
- Add printers to CUPS: Use System Preferences or `lpadmin` command
- Verify printers are available: Run `lpstat -p`

### "No such file or directory" error
**Solution:**
- Use absolute file paths (e.g., `/Users/john/file.pdf`, not `~/file.pdf` or `./file.pdf`)
- Ensure the file exists and you have read permissions
- Check file extension matches supported types

### File validation fails
**Solution:**
- Verify file type is in the supported list: PDF, TXT, MD, CSV, DOCX, PNG, JPEG
- Ensure file is not corrupted
- Check file extension is lowercase (or it will be normalized)

### "Printer not found" in non-interactive mode
**Solution:**
- Verify the printer name exactly matches system printer names
- List available printers: `lpstat -p -d`
- Use the exact printer name in the command (case-sensitive)

### Print job fails to submit
**Solution:**
- Verify printer is online: `lpstat -p -d`
- Check printer has sufficient disk space
- Ensure you have permission to print to the selected printer
- Try printing a test page: `lp -d printer_name /etc/hosts`

### DOCX conversion fails ("Failed to convert DOCX to PDF")
**Solution:**
- Verify LibreOffice is installed: `which libreoffice` or `which soffice`
- Install LibreOffice:
  ```bash
  # macOS
  brew install libreoffice
  
  # Ubuntu/Debian
  sudo apt-get install libreoffice
  
  # RHEL/CentOS
  sudo yum install libreoffice
  ```
- Ensure DOCX file is not corrupted
- Try converting manually: `libreoffice --headless --convert-to pdf document.docx`

### Markdown conversion fails ("Failed to convert Markdown to PDF")
**Solution:**
- Verify Pandoc is installed: `which pandoc`
- Install Pandoc:
  ```bash
  # macOS
  brew install pandoc
  
  # Ubuntu/Debian
  sudo apt-get install pandoc
  
  # RHEL/CentOS
  sudo yum install pandoc
  ```
- Try converting manually: `pandoc README.md -o README.pdf`

### Old Bash compatibility issues
**Solution:**
- Verify your Bash version: `bash --version`
- If using Bash < 3.2, update Bash:
  ```bash
  # macOS
  brew install bash
  
  # Ubuntu/Debian
  sudo apt-get install bash
  
  # RHEL/CentOS
  sudo yum install bash
  ```
- Change the shebang line in the `println` script if needed:
  ```bash
  # For systems with different shell location
  #!/usr/bin/env bash
  # or
  #!/bin/sh  # For POSIX shell compatibility (limited features)
  ```

## Advanced: Viewing & Managing Print Queue

Check the status of print jobs:
```bash
# View all jobs
lpstat -o

# View jobs for specific printer
lpstat -o -d printer_name

# Cancel a print job
cancel job_id

# Cancel all jobs for a printer
cancel -P printer_name
```

## Version History

### v3.1.0 - Current
- ✨ Added DOCX support with automatic conversion to PDF
- ✨ Added Markdown support with automatic conversion to PDF
- 🔧 Improved Bash compatibility (3.2+, removed `bc` dependency)
- 🐛 Fixed duplicate "both sides" prompts in interactive mode
- 🐛 Fixed interactive workflow order (copies prompt before summary)
- 🔧 Better error handling and validation
- 🔧 Automatic cleanup of temporary conversion files
- 📚 Updated documentation with conversion examples

### v3.0.0
- Initial release with duplex, copies, collate, and paper size support
- Interactive and non-interactive modes
- Basic file format support (PDF, TXT, MD, CSV, PNG, JPEG)

## Installation Updates & Maintenance

### Updating PrintLN

If you installed PrintLN globally with `sudo cp`:

```bash
# Navigate to your println repository directory
cd /path/to/println

# Pull the latest changes
git pull origin main

# Update the global installation
sudo cp println /usr/local/bin/println

# Verify the update
println -v
```

If you used an alias instead:

```bash
# Navigate to your println repository directory
cd /path/to/println

# Pull the latest changes
git pull origin main

# The alias automatically points to the updated version
# Verify the update
println -v
```

### Uninstalling PrintLN

**If installed globally:**

```bash
# Remove from /usr/local/bin
sudo rm /usr/local/bin/println

# Optionally, remove the cloned repository
rm -rf /path/to/println
```

**If using an alias:**

```bash
# Remove the alias from ~/.bashrc or ~/.zshrc
# Edit the file and delete this line:
# alias println="/path/to/your/println"

# Then reload your shell
source ~/.bashrc  # or source ~/.zshrc

# Optionally, remove the cloned repository
rm -rf /path/to/println
```

**Remove logs (optional):**

```bash
# Remove all PrintLN logs
rm -rf ~/println_logs
```

## Architecture

### Pure Bash Implementation
- **Minimal external dependencies** - only requires CUPS and standard Unix utilities
- **Fast execution** - instant startup, no interpreters to load
- **Portable** - works across different Unix-like systems
- **Maintainable** - single script, easy to understand and modify
- **Backward compatible** - supports older Bash versions (3.2+)

### Key Components
1. **Printer Detection** - Uses `lpstat` to list available printers
2. **File Validation** - Checks file existence and supported type
3. **Document Conversion** - Converts DOCX/Markdown to PDF automatically
4. **Interactive Prompts** - Simple `read` commands for user input
5. **Non-Interactive Processing** - Argument parsing for automated workflows
6. **Print Submission** - Uses `lp` command with CUPS options
7. **Error Handling** - Comprehensive validation and user feedback
8. **Logging** - Error logs stored in `~/println_logs/`
9. **Cleanup** - Automatic removal of temporary conversion files

## Configuration

PrintLN uses system printer settings by default. Print jobs are submitted with:
- **Media**: User-selected paper size (default: A4)
- **Sides**: User-selected duplex mode (default: Two-sided long-edge)
- **Color**: User-selected color mode (default: Black & White)
- **Collate**: User-selected collation (default: Enabled)

To customize print options, edit the `lp_options` variable in the `println` script or use command-line options in non-interactive mode.

## Performance & Conversion

### File Conversion Workflow
- **DOCX files**: Converted to PDF via LibreOffice in a temporary directory
- **Markdown files**: Converted to PDF via Pandoc in a temporary directory
- **Temporary files**: Automatically cleaned up after printing
- **Conversion errors**: Clear error messages guide you to install required tools

### Performance Notes
- First-time conversion may take 2-5 seconds (LibreOffice startup)
- Subsequent prints are faster due to caching
- Large documents (100+ MB) may require more conversion time

## License

This project is licensed under the BSD 3-Clause License - see the [LICENSE](LICENSE) file for details.

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## Support

For issues, feature requests, or questions, please visit:
https://github.com/live-by-unix/println/issues

---

**Made with ❤️ by live-by-unix**
