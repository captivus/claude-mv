# claude-mv

A utility script to safely rename project directories while preserving their Claude Code history.

## Overview

`claude-mv` is a Bash script that handles the often-overlooked task of renaming a project directory along with its associated Claude Code history folder. When you rename a project directory, Claude Code loses track of the project history because it stores history based on the directory path. This script solves that problem by renaming both the project and its history in one operation.

## Features

- **Atomic renaming**: Renames both project directory and Claude history folder together
- **Safety checks**: Validates paths and prevents accidental overwrites
- **Dry-run mode**: Preview changes before executing them
- **Smart CWD handling**: Automatically handles renaming when you're inside the directory
- **Color-coded output**: Clear visual feedback (can be disabled)
- **Cross-platform**: Works with readlink, realpath, or fallback methods

## Installation

1. Download the script:
```bash
curl -O https://raw.githubusercontent.com/yourusername/claude-mv/main/claude-mv
```

2. Make it executable:
```bash
chmod +x claude-mv
```

3. Optionally, move it to your PATH:
```bash
sudo mv claude-mv /usr/local/bin/
```

## Usage

### Basic usage
```bash
claude-mv OLD_PATH NEW_PATH
```

### Options
- `-n, --dry-run` - Show what would be done without making changes
- `-y, --yes` - Skip confirmation prompt
- `--no-color` - Disable ANSI color output
- `-h, --help` - Display help message

### Examples

Rename a project with confirmation:
```bash
claude-mv /home/alice/code/old-project /home/alice/code/new-project
```

Preview changes without executing:
```bash
claude-mv -n ~/projects/api-v1 ~/projects/api-v2
```

Skip confirmation (useful in scripts):
```bash
claude-mv -y ./my-app ./my-awesome-app
```

## How It Works

Claude Code stores project history in `~/.claude/projects/` using encoded directory paths. The script:

1. **Normalizes paths** to absolute paths using readlink/realpath
2. **Encodes paths** using Claude's naming convention (slashes and underscores become hyphens)
3. **Validates** that source exists and destination doesn't
4. **Renames** both the project directory and its history folder
5. **Handles edge cases** like being inside the directory during rename

### Path Encoding

The script mimics Claude's path encoding:
- `/home/user/my_project` → `-home-user-my-project`
- All `/` become `-`
- All `_` become `-`
- Leading slash becomes leading dash

## Requirements

- Bash 4.0 or higher
- Standard Unix utilities (mv, sed, tput)
- Either `readlink` (from coreutils) or `realpath`

## Exit Status

- `0` - Success
- Non-zero - Error occurred (validation failure, permission denied, etc.)

## Safety Features

- **Path validation**: Ensures source exists and destination doesn't
- **Identity check**: Prevents renaming to the same path
- **Confirmation prompt**: Requires user confirmation (unless `-y` flag used)
- **History preservation**: Warns if history directory already exists at destination
- **CWD safety**: Automatically changes directory if renaming current working directory

## Limitations

- Only works with Claude Code projects that store history in `~/.claude/projects/`
- Cannot merge existing histories if destination history already exists
- Requires appropriate filesystem permissions for both directories

## Troubleshooting

**"Source directory not found"**: Verify the path exists and you have read permissions

**"Destination already exists"**: The target path is already in use; choose a different name

**"No Claude history found"**: Normal if the project hasn't been opened in Claude Code yet

**Colors not working**: Check if your terminal supports colors, or use `--no-color`

## License

This script is provided as-is for utility purposes. Feel free to modify and distribute.

## Contributing

Improvements and bug reports are welcome! Please test changes thoroughly as this script modifies filesystem structures.

## Author

Created for the Claude Code community to solve the common problem of preserving project history during directory renames.