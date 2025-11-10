# Universal Ctags Binary Releases

This repository provides pre-built binaries of [Universal Ctags](https://github.com/universal-ctags/ctags) for easy installation via [mise](https://mise.jdx.dev/) (formerly rtx).

## Features

- Automated daily checks for new Universal Ctags releases
- Pre-built binaries for multiple platforms:
  - Linux x86_64
  - Linux ARM64 (aarch64)
  - macOS x86_64 (Intel)
  - macOS ARM64 (Apple Silicon)
- Built with support for:
  - JSON output (`--output-format=json`)
  - YAML output (`--output-format=yaml`)
  - XML output (`--output-format=xml`)
  - Seccomp sandbox (Linux builds only)

## Installation

### Using mise (Recommended)

#### Global installation

Add to your `~/.config/mise/config.toml`:

```toml
[tools]
"github:ompugao/ctags-release" = "latest"
```

Then run:
```bash
mise install
```

#### Project-specific installation

In your project directory, create or update `.mise.toml`:

```toml
[tools]
"github:ompugao/ctags-release" = "latest"
```

Then run:
```bash
mise install
```

#### Direct installation

```bash
# Install the latest version
mise use -g github:ompugao/ctags-release@latest

# Install a specific version
mise use -g github:ompugao/ctags-release@p6.1.20241110.0
```

### Manual Installation

Download the appropriate binary for your platform from the [Releases](https://github.com/ompugao/ctags-release/releases) page:

```bash
# Example for Linux x86_64
VERSION=p6.1.20241110.0  # Replace with desired version
PLATFORM=linux-x86_64     # or darwin-x86_64, darwin-aarch64, linux-aarch64

# Download and extract
curl -L "https://github.com/ompugao/ctags-release/releases/download/${VERSION}/ctags-${VERSION}-${PLATFORM}.tar.gz" | tar xz

# Move to PATH
sudo mv ctags /usr/local/bin/

# Verify installation
ctags --version
```

## Supported Platforms

| Platform | Architecture | Binary Name | Minimum Requirements |
|----------|-------------|-------------|---------------------|
| Linux | x86_64 | `ctags-*-linux-x86_64.tar.gz` | glibc 2.31+ (Ubuntu 20.04+, Debian 11+, RHEL 8+) |
| Linux | ARM64 | `ctags-*-linux-aarch64.tar.gz` | glibc 2.31+ |
| macOS | x86_64 (Intel) | `ctags-*-darwin-x86_64.tar.gz` | macOS 12+ |
| macOS | ARM64 (Apple Silicon) | `ctags-*-darwin-aarch64.tar.gz` | macOS 14+ |

## Compatibility Notes

- **Linux binaries** are built on Ubuntu 20.04 LTS to ensure compatibility with older systems
- Requires **glibc 2.31** or newer (check with `ldd --version`)
- Uses static linking where possible to minimize dependency issues
- If you encounter library errors on Linux, verify your glibc version:
  ```bash
  ldd --version | head -1
  ```

## Usage

Once installed, you can use ctags as normal:

```bash
# Generate tags for current directory
ctags -R .

# Generate tags with JSON output
ctags --output-format=json -R .

# Generate tags for specific languages
ctags --languages=Python,JavaScript -R .

# See all options
ctags --help
```

## How It Works

This repository uses GitHub Actions to:

1. Check the [universal-ctags/ctags](https://github.com/universal-ctags/ctags) repository daily for new releases
2. When a new release is detected, automatically build binaries for all supported platforms
3. Create a GitHub release with the pre-built binaries
4. Tag the release with the same version as the upstream Universal Ctags

The mise plugin configuration allows mise to:
- List available versions from this repository's releases
- Download the appropriate binary for your platform
- Install it in the mise-managed directory

## Build Configuration

The binaries are built with the following configuration:

**Linux:**
```bash
./configure \
  --prefix=/usr/local \
  --enable-json \
  --enable-yaml \
  --enable-xml \
  --enable-seccomp
```

**macOS:**
```bash
./configure \
  --prefix=/usr/local \
  --enable-json \
  --enable-yaml \
  --enable-xml
```

## Updating

To get the latest version when using mise:

```bash
# Update to the latest version
mise upgrade github:ompugao/ctags-release

# Or reinstall
mise uninstall github:ompugao/ctags-release
mise install github:ompugao/ctags-release@latest
```

## Troubleshooting

### Binary not found after installation

Make sure mise is properly set up in your shell:

```bash
# For bash
echo 'eval "$(mise activate bash)"' >> ~/.bashrc

# For zsh
echo 'eval "$(mise activate zsh)"' >> ~/.zshrc

# For fish
echo 'mise activate fish | source' >> ~/.config/fish/config.fish
```

### Permission denied when running ctags

Make sure the binary has execute permissions:

```bash
chmod +x $(which ctags)
```

### Version not available

The GitHub Action runs daily to check for new releases. If a very recent release isn't available yet, you can:
1. Wait for the next automated build (runs daily at 2 AM UTC)
2. Trigger a manual build by going to the [Actions tab](https://github.com/ompugao/ctags-release/actions) and running the workflow manually

## Contributing

Issues and pull requests are welcome! If you encounter any problems or have suggestions for improvements, please open an issue.

## License

The build scripts and GitHub Actions workflow in this repository are provided as-is. Universal Ctags itself is licensed under GPL-2.0. See the [Universal Ctags repository](https://github.com/universal-ctags/ctags) for more information.

## Acknowledgments

- [Universal Ctags](https://github.com/universal-ctags/ctags) maintainers and contributors
- [mise](https://mise.jdx.dev/) for the excellent version management tool