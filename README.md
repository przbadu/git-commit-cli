# Git Commit CLI

A Ruby command-line tool that uses AI to generate conventional commit messages based on your git changes.

## Features

- Generates high-quality commit messages using AI
- Supports multiple LLM providers via the RubyLLM gem
- Follows conventional commit format
- Includes issue/ticket numbers in commit messages
- Analyzes both staged and unstaged changes
- Customizable system prompt for different commit styles

## Installation

### Prerequisites

- Ruby 2.7 or higher
- Git
- RubyLLM gem

### Install Dependencies

```bash
gem install ruby_llm thor
```

### Install Git Commit CLI

1. Clone the repository:
   ```bash
   git clone https://github.com/przbadu/git-commit-cli.git
   cd git-commit-cli
   ```

2. Make the script executable:
   ```bash
   chmod +x git-commit.rb
   ```

3. Install globally:
   ```bash
   ./git-commit.rb install
   ```

This will create a symlink to the script in `/usr/local/bin/git-commit`, making it available globally.

## Configuration

Before using the CLI, you need to configure it to use your preferred LLM provider:

```bash
git-commit setup
```

This interactive setup will:
1. Ask which LLM provider you want to use (OpenAI, Anthropic, Ollama, etc.)
2. Configure the model and necessary API keys
3. Set up a system prompt for generating commit messages

You can also use command-line options for non-interactive setup:

```bash
git-commit setup --provider openai --model gpt-4 --api-key YOUR_API_KEY
```

### Available Providers

To see which LLM providers are available on your system:

```bash
git-commit providers
```

## Usage

### Generate a Commit Message

```bash
git-commit commit
```

This will:
1. Check for staged changes
2. If none are found, ask if you want to stage unstaged changes
3. Analyze the changes using your configured LLM
4. Show the generated commit message
5. Ask for confirmation before committing

### Include an Issue Number

```bash
git-commit commit --issue 123
```

This will include the issue number in the conventional commit format:

```
feat(component): #123 add new feature

- Detailed description of changes
- Reason for changes

Closes #123
```

### Add Custom Instructions

```bash
git-commit commit --issue 456 --prompt "Focus on the performance improvements"
```

### Stage All Changes

```bash
git-commit commit --stage-all
```

## Commit Message Format

The CLI generates commit messages following the conventional commit format:

```
type(scope): #issue_number short descriptive title

- Brief description of what changed
- Why it was changed (if not obvious)
- Any side effects or important notes

Closes #issue_number
```

### Commit Types

- `feat`: New feature or functionality
- `fix`: Bug fix or error correction
- `perf`: Performance improvements
- `refactor`: Code restructuring without changing functionality
- `style`: Formatting, spacing, semicolons (no code change)
- `docs`: Documentation updates
- `test`: Adding or updating tests
- `chore`: Maintenance tasks, dependency updates
- `build`: Build system or external dependency changes
- `ci`: CI/CD configuration changes
- `revert`: Reverting a previous commit

## Customization

### Custom System Prompt

You can provide your own system prompt during setup:

```bash
git-commit setup --system-prompt "Your custom prompt here"
```

Or update the existing configuration:

```bash
vim ~/.git_commit_cli/config.yml
```

## Troubleshooting

### LLM Configuration Issues

If you experience issues with your LLM provider:

1. Run the test during setup to verify your configuration
2. Check the API key and base URL
3. Make sure the selected model is available with your provider

### Installation Problems

If you encounter permission issues during installation:

```bash
sudo ./git-commit.rb install
```

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the LICENSE file for details.
