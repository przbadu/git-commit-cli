# Git Commit CLI

An intelligent Git commit message generator powered by AI. Generate meaningful, well-structured commit messages using various LLM providers including OpenAI, Anthropic, Google Gemini, and local models via Ollama.

## Features

- **Multi-Provider Support**: Works with OpenAI, Anthropic, Google Gemini, DeepSeek, Mistral, Perplexity, Ollama, and more
- **Conventional Commits**: Generates messages following the conventional commit format
- **Auto-Detection**: Automatically detects available LLM providers from environment variables
- **Customizable Prompts**: Configure your own system prompt for commit message generation
- **Zero Configuration**: Works out-of-the-box with just environment variables
- **Smart Staging**: Optionally stage all changes before committing
- **Issue Linking**: Include issue/ticket numbers in commit messages
- **Model Selection**: Choose specific models on-the-fly

## Installation

### Prerequisites

- Ruby 2.7 or higher
- Git
- [RubyLLM gem](https://rubyllm.com)

### Install Dependencies

```bash
gem install ruby_llm thor
```

### Install the CLI Tool

1. Clone or download the `git-commit` script
2. Make it executable:
   ```bash
   chmod +x git-commit
   ```
3. Install globally (optional):
   ```bash
   sudo ./git-commit install
   ```

## Configuration

### Setting Up LLM Providers

The tool automatically detects LLM providers based on environment variables. Simply export the API key for your preferred provider:

#### OpenAI
```bash
export OPENAI_API_KEY='sk-...'
```

#### Anthropic (Claude)
```bash
export ANTHROPIC_API_KEY='sk-ant-...'
```

#### Google Gemini
```bash
export GEMINI_API_KEY='...'
```

#### DeepSeek
```bash
export DEEPSEEK_API_KEY='...'
```

#### Mistral
```bash
export MISTRAL_API_KEY='...'
```

#### Perplexity
```bash
export PERPLEXITY_API_KEY='...'
```

#### Ollama (Local Models)
Install Ollama from [https://ollama.ai](https://ollama.ai) and pull a model:
```bash
ollama pull llama3.1
```

If running Ollama on a different host/port:
```bash
export OLLAMA_API_BASE='http://your-server:11434'
```

### AWS Bedrock
```bash
export AWS_ACCESS_KEY_ID='...'
export AWS_SECRET_ACCESS_KEY='...'
export AWS_REGION='us-west-2'
```

### Custom OpenAI-Compatible Endpoints
For Azure OpenAI or other compatible APIs:
```bash
export OPENAI_API_BASE='https://your-endpoint.openai.azure.com'
export OPENAI_API_KEY='your-api-key'
```

## Usage

### Basic Usage

Generate a commit message for staged changes:
```bash
git add .
git-commit commit
```

Or with environment variable inline:
```bash
OPENAI_API_KEY='sk-...' git-commit commit
```

### Stage All Changes and Commit
```bash
git-commit commit -a
# or
git-commit commit --stage-all
```

### Use a Specific Model
```bash
git-commit commit --model gpt-4
git-commit commit --model claude-3-opus
git-commit commit --model llama3.1
```

### Include Issue Number
```bash
git-commit commit --issue 123
# or
git-commit commit -i 456
```

### Add Custom Instructions
```bash
git-commit commit --prompt "Focus on the performance improvements"
```

### Combine Options
```bash
git-commit commit -a --model gpt-4 --issue 789 --prompt "Emphasize security fixes"
```

## Commands

### `commit` - Generate and commit
Generate an AI-powered commit message and commit the changes.

**Options:**
- `-a, --stage-all` - Stage all unstaged changes before committing
- `-i, --issue NUMBER` - Include issue/ticket number in the commit message
- `-m, --model MODEL` - Use a specific model (e.g., gpt-4, claude-3-opus)
- `--prompt TEXT` - Additional instructions for the LLM

### `info` - Display configuration and available models
Show current configuration and list all available models from configured providers.

```bash
git-commit info
```

### `setup` - Configure system prompt
Customize the system prompt used for generating commit messages.

```bash
# Interactive setup
git-commit setup

# Set custom prompt directly
git-commit setup --system-prompt "Your custom prompt here"

# Reset to default prompt
git-commit setup --reset
```

### `install` - Install globally
Install the git-commit command to `/usr/local/bin` for system-wide access.

```bash
sudo git-commit install
```

## Commit Message Format

The tool generates commit messages following the Conventional Commits specification:

```
type(scope): #issue_number short descriptive title

- Brief description of what changed
- Why it was changed (if not obvious)
- Any side effects or important notes

Closes #issue_number
```

### Commit Types
- `feat` - New feature or functionality
- `fix` - Bug fix or error correction
- `perf` - Performance improvements
- `refactor` - Code restructuring without changing functionality
- `style` - Formatting, spacing, semicolons (no code change)
- `docs` - Documentation updates
- `test` - Adding or updating tests
- `chore` - Maintenance tasks, dependency updates
- `build` - Build system or external dependency changes
- `ci` - CI/CD configuration changes
- `revert` - Reverting a previous commit

## Examples

### Example 1: Simple commit with OpenAI
```bash
$ git add src/
$ OPENAI_API_KEY='sk-...' git-commit commit

Analyzing changes...
Staged files: src/main.rb, src/helper.rb

Generating commit message...

Generated commit message:
------------------------
refactor(src): improve code organization and error handling

- Extract helper methods to separate module
- Add proper error handling for API calls
- Improve code readability with better naming

------------------------

Use this commit message? (y/n) y
Changes committed successfully!
```

### Example 2: Feature with issue number
```bash
$ git-commit commit -a --issue 234 --model claude-3-opus

Staged all changes.

Analyzing changes...
Staged files: app/controllers/checkout_controller.rb, app/views/checkout/guest.html.erb

Generating commit message...

Generated commit message:
------------------------
feat(checkout): #234 add guest checkout option

- Allow users to complete purchase without account
- Store guest orders with email reference
- Add order lookup by email and order ID

Closes #234
------------------------

Use this commit message? (y/n) y
Changes committed successfully!
```

### Example 3: Using local Ollama
```bash
$ git-commit commit --model llama3.1

Analyzing changes...
Staged files: README.md

Generating commit message...

Generated commit message:
------------------------
docs(README): update installation instructions

- Add prerequisites section
- Include Ollama setup steps
- Fix broken links to documentation

------------------------

Use this commit message? (y/n) y
Changes committed successfully!
```

## Advanced Configuration

### Proxy Support
```bash
export HTTP_PROXY='http://proxy.company.com:8080'
# or with authentication
export HTTP_PROXY='http://user:pass@proxy.company.com:8080'
```

### Debug Mode
```bash
export DEBUG=true
git-commit commit
```

### Custom Default Model
```bash
export DEFAULT_LLM_MODEL='gpt-4'
```

## Troubleshooting

### No LLM provider configured
If you see this error, make sure you've exported at least one provider's API key:
```bash
echo $OPENAI_API_KEY  # Should show your key (or use other provider variables)
```

### Ollama connection failed
Ensure Ollama is running:
```bash
ollama serve
```

### API rate limits
If you hit rate limits, try:
1. Using a different provider
2. Waiting a moment before retrying
3. Using a local model with Ollama

### Model not found
Check available models:
```bash
git-commit info
```

## Contributing

Contributions are welcome! Please feel free to submit issues or pull requests.

## License

MIT License - feel free to use this tool in your projects.

## Acknowledgments

- Powered by [RubyLLM](https://rubyllm.com) - A unified Ruby interface for multiple LLM providers
- Follows [Conventional Commits](https://www.conventionalcommits.org/) specification

## Support

For issues, questions, or suggestions, please open an issue on the project repository.