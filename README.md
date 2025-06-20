# WordPress VIP Filesystem Scanner

A comprehensive bash script to scan WordPress plugins and themes for filesystem functions that are incompatible with WordPress VIP hosting environments.

## 🚀 Features

- **Recursive scanning** of PHP files in any directory
- **Comprehensive function detection** - checks 50+ incompatible functions
- **Detailed reporting** with file paths and line numbers
- **Color-coded output** for better readability
- **CI/CD friendly** with proper exit codes
- **Zero dependencies** - pure bash script

## 🔍 What it detects

The scanner identifies VIP-incompatible functions including:

- **File operations**: `fopen()`, `fwrite()`, `file_put_contents()`, `file_get_contents()`
- **Directory operations**: `mkdir()`, `rmdir()`, `scandir()`, `opendir()`
- **File permissions**: `chmod()`, `is_writable()`, `is_readable()`
- **File manipulation**: `unlink()`, `rename()`, `copy()`
- **Temporary files**: `tempnam()`, `tmpfile()`
- **System execution**: `exec()`, `system()`, `shell_exec()`
- And many more...

## 📋 Requirements

- **macOS** or **Linux** environment
- **Bash** 4.0+ (standard on most systems)
- Read access to the directories you want to scan

## ⚡ Quick Start

### Installation

```bash
# Clone the repository
git clone https://github.com/vikramsaini/wp-vip-filesystem-scanner.git
cd wp-vip-filesystem-scanner

# Make the script executable
chmod +x vip-scanner.sh
```

### Usage

```bash
# Scan a plugin directory
./vip-scanner.sh /path/to/your/plugin

# Scan a theme directory
./vip-scanner.sh /path/to/your/theme

# Example with WordPress plugin
./vip-scanner.sh /var/www/html/wp-content/plugins/my-plugin
```

## 📖 Example Output

```bash
$ ./vip-scanner.sh /path/to/plugin

Checking for VIP filesystem incompatible functions in: /path/to/plugin
==================================================
Scanning PHP files...

✗ /path/to/plugin/includes/file-handler.php
  → file_put_contents found on line(s): 45,67
  → mkdir found on line(s): 23

✗ /path/to/plugin/admin/settings.php
  → file_get_contents found on line(s): 156

==================================================
SCAN COMPLETE

Total PHP files scanned: 47
✗ Files with VIP incompatible functions: 2

Functions found:
  - file_get_contents
  - file_put_contents
  - mkdir

Note: Please review these files and replace incompatible functions with VIP-approved alternatives.
Refer to WordPress VIP documentation for approved filesystem functions.
```

## 🔧 CI/CD Integration

The script returns appropriate exit codes:
- `0`: No incompatible functions found
- `1`: Incompatible functions detected

### GitHub Actions Example

```yaml
name: VIP Compatibility Check
on: [push, pull_request]

jobs:
  vip-scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Run VIP Scanner
        run: |
          chmod +x vip-scanner.sh
          ./vip-scanner.sh ./your-plugin-directory
```

## 🛠️ WordPress VIP Alternatives

When incompatible functions are found, consider these VIP-approved alternatives:

| Incompatible Function | VIP Alternative |
|----------------------|-----------------|
| `file_get_contents()` | `wp_remote_get()` for URLs, `WP_Filesystem` for files |
| `file_put_contents()` | `WP_Filesystem::put_contents()` |
| `fopen()`, `fwrite()` | `WP_Filesystem` methods |
| `mkdir()` | `wp_mkdir_p()` |
| `unlink()` | `WP_Filesystem::delete()` |
| `is_writable()` | `WP_Filesystem::is_writable()` |

For complete documentation, visit: [WordPress VIP Filesystem API](https://docs.wpvip.com/technical-references/vip-codebase/filesystem-api/)

## 🤝 Contributing

Contributions are welcome! Please feel free to submit issues, feature requests, or pull requests.

### Adding New Functions

To add new incompatible functions to the scanner:

1. Edit the `INCOMPATIBLE_FUNCTIONS` array in `vip-scanner.sh`
2. Add the function with proper syntax (include the opening parenthesis)
3. Test your changes
4. Submit a pull request

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 👨‍💻 Author

**Vikram Saini**

- GitHub: [@vikram-saini](https://github.com/vikram-saini)

## ⭐ Support

If this tool helps you, please consider giving it a star on GitHub!

## 🔗 Related Resources

- [WordPress VIP Documentation](https://docs.wpvip.com/)
- [WordPress VIP Code Review Guidelines](https://docs.wpvip.com/technical-references/code-review/)
- [WordPress Filesystem API](https://developer.wordpress.org/apis/wp-filesystem/)

---

**Disclaimer**: This tool is not officially affiliated with WordPress VIP. Always refer to the official WordPress VIP documentation for the most up-to-date compatibility requirements.
