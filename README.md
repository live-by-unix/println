# PrintLN - Bash CLI Print Tool

A lightweight, production-ready Bash CLI tool for printing documents to network and local printers with an interactive workflow. No Node.js dependencies required!

## Features

- 🖨️ **System Printer Integration** - Automatically detects available printers via CUPS
- 📄 **Multiple File Formats** - Supports PDF, TXT, MD, CSV, DOCX, PNG, and JPEG
- 🎨 **Color Mode Selection** - Choose between Black & White and Color printing
- ✅ **Interactive Workflow** - User-friendly prompts and confirmations
- 🚀 **Lightweight** - Pure Bash, no external dependencies
- 💻 **Cross-Platform** - Works on macOS, Linux, and Unix systems

## Requirements

- Bash 4.0+
- CUPS (Common Unix Printing System) installed and configured
- `lp` command-line utility (comes with CUPS)
- `lpstat` utility for printer detection
- `bc` for calculations (standard on most systems)

## Installation

### Quick Install

```bash
# Clone the repository
git clone https://github.com/live-by-unix/println.git
cd println

# Make the script executable
chmod +x println

# Optional: Install globally
sudo cp println /usr/local/bin/println # Do this IN the println dir.
```
If you want to use it as a command but don't have sudo, go to your ./bashrc or ./zshrc and put this:

```bash
alias println="/path/to/your/println"
```
Also, if you prefer to clone a release, go ahead.
### Verify Installation

```bash
# Check if println is accessible
println -v

# Output:
# println v1.0.0
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

### Step 4: Review Summary
```
📋 Print Job Summary:
──────────────────────────────────────────────
Printer:     Brother_HL_L8360CDW
File Path:   /Users/john/documents/report.pdf
File Name:   report.pdf
File Size:   245.32 KB
Color Mode:  Black & White
──────────────────────────────────────────────
```

### Step 5: Confirm & Submit
```
Proceed with print job? (y/N): y

ℹ️  Submitting print job...

✅ Print job submitted successfully!
📌 Printer: Brother_HL_L8360CDW
📄 File: report.pdf
```

## Supported File Types

- **Documents**: PDF, TXT, MD (Markdown), CSV, DOCX (Word)
- **Images**: PNG, JPEG

## Examples

### Print a PDF in Black & White
```bash
$ println print
ℹ️  Scanning for available printers...

  1. Office_Printer
  2. Personal_Printer

Select a printer (enter number): 1
✅ Selected printer: Office_Printer

Enter absolute path of file to print: /home/user/report.pdf
✅ File validated: report.pdf

Select color mode:
  1. Black & White
  2. Color

Enter choice (1 or 2): 1
✅ Selected color mode: Black & White

📋 Print Job Summary:
──────────────────────────────────────────────
Printer:     Office_Printer
File Path:   /home/user/report.pdf
File Name:   report.pdf
File Size:   512.45 KB
Color Mode:  Black & White
──────────────────────────────────────────────

Proceed with print job? (y/N): y

ℹ️  Submitting print job...

✅ Print job submitted successfully!
📌 Printer: Office_Printer
📄 File: report.pdf
```

### Print an Image in Color
```bash
$ println print
# ... follow the prompts ...
# Select color mode: 2 (Color)
# ... confirm and print ...
```

### Cancel a Print Job
```bash
$ println print
# ... follow the prompts ...
Proceed with print job? (y/N): n

❌ Print job cancelled.
```

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
- Check file extension is lowercase

### Print job fails to submit
**Solution:**
- Verify printer is online: `lpstat -p -d`
- Check printer has sufficient disk space
- Ensure you have permission to print to the selected printer
- Try printing a test page: `lp -d printer_name /etc/hosts`

## Advanced: Viewing Print Queue

To check the status of print jobs:
```bash
# View all jobs
lpstat -o

# View jobs for specific printer
lpstat -o -d printer_name

# Cancel a print job
cancel job_id
```

## Architecture

### Pure Bash Implementation
- **No external dependencies** beyond CUPS (standard on macOS/Linux)
- **Fast execution** - instant startup
- **Portable** - works across different Unix-like systems
- **Maintainable** - single script, easy to understand

### Key Components
1. **Printer Detection** - Uses `lpstat` to list available printers
2. **File Validation** - Checks file existence and supported type
3. **Interactive Prompts** - Simple `read` commands for user input
4. **Print Submission** - Uses `lp` command with CUPS options
5. **Error Handling** - Comprehensive validation and user feedback

## Configuration

PrintLN uses system printer settings by default. Print jobs are submitted with:
- **Media**: A4 (standard)
- **Sides**: Two-sided long-edge (duplex)
- **Color**: User-selected (Black & White or Color)

To customize print options, edit the `lp_options` variable in the `println` script.

## License

This project is licensed under the BSD 3-Clause License - see the [LICENSE](LICENSE) file for details.

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## Support

For issues, feature requests, or questions, please visit:
https://github.com/live-by-unix/println/issues

---

**Made with ❤️ by live-by-unix**
