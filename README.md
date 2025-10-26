# OASIS: A Comprehensive Framework for Evaluating AI Agent Safety and Security in Multi-Turn Interactions

## Overview

OASIS is a comprehensive evaluation framework designed to assess the safety and security of AI agents across multiple dimensions. This system provides automated testing capabilities with configurable parameters for different test levels, difficulties, and execution modes, supporting various AI models and deployment scenarios.

## Key Features

- **Multi-Model Support**: Compatible with various AI models including GPT5, Qwen, and others
- **Comprehensive Safety Assessment**: Evaluates agent behavior across multiple safety dimensions
- **Configurable Test Parameters**: Flexible test levels (L0-L3), difficulties (Low/Medium/High), and test counts
- **Real-World Tool Integration**: Tests agents with realistic tool-calling scenarios
- **Resume Functionality**: Ability to resume from existing test trajectories
- **Concurrent Execution**: Support for parallel test execution with configurable worker threads
- **Detailed Analytics**: Comprehensive metrics and trajectory analysis
- **API Health Checks**: Automatic verification of API connectivity before testing
- **Enhanced Logging**: Detailed logging system with multiple output formats

## System Architecture

### Core Components

- **Test Runner** (`evaluation/test_runner.py`): Main orchestration engine
- **Multi-Turn Agent** (`evaluation/multi_turn_agent.py`): Agent interaction handler
- **Safety Analyzer** (`evaluation/safety_analyzer.py`): Safety assessment engine
- **Trajectory Saver** (`evaluation/trajectory_saver.py`): Test result persistence
- **Tool Registry** (`tools/tool_registry.py`): Tool management system

### Test Modes

1. **Real-World Mode**: Tests agents with realistic tool interactions
2. **Direct Evaluation Mode**: Direct assessment of agent responses

## Prerequisites



### Required Dependencies
```bash
pip install openai aiohttp pandas numpy matplotlib seaborn
```

### Required Files
- `run_simple_test.py`: Main Python test runner script
- `merged_tasks_updated.jsonl`: Test dataset file
- Tool configuration files in `tools/` directory
- Valid API credentials for target models

## Installation

1. **Clone the repository**:
```bash
git clone https://github.com/open-compass/OASIS.git
cd OASIS
```

2. **Install dependencies**:
```bash
pip install -r requirements.txt
```

3. **Configure API credentials**:
```bash
export OPENAI_API_KEY="your-api-key-here"
export OPENAI_BASE_URL="optional-custom-api-url"
```

## Configuration

### Model Configuration
```python
MODEL_NAME="gpt-5"              # Model identifier
API_KEY="your-api-key"          # API authentication
API_URL="optional-custom-url"   # Custom API endpoint
BASE_OUTPUT_DIR="./results"     # Output directory
DATASET="dataset.jsonl"       # Test dataset path
```

### Test Parameters
- **MAX_ITERATIONS**: 10-30 (Maximum iterations per test)
- **TEMPERATURE**: 0.0-2.0 (Model creativity parameter)
- **TIMEOUT**: 300-1200 seconds (Request timeout)
- **MAX_WORKERS**: 1-8 (Concurrent execution workers)
- **RETRY_COUNT**: 5-20 (Failure retry attempts)

### Test Levels
- **L0**: Basic functionality tests
- **L1**: Standard safety assessments
- **L2**: Advanced security evaluations
- **L3**: Complex multi-turn scenarios

### Difficulty Levels
- **Low**: Simple, straightforward test cases
- **Medium**: Moderate complexity scenarios
- **High**: Complex, multi-faceted challenges

## Usage

### Basic Command Syntax
```bash
python run_simple_test.py [OPTIONS]
```

### Essential Parameters

#### Model Selection
```bash
--model "gpt-5"                    # Specify AI model
--api-key "your-key"              # API authentication
--base-url "custom-url"           # Custom API endpoint
```

#### Test Configuration
```bash
--mode "real_world"               # Test mode (real_world/direct_eval)
--dataset "dataset.jsonl"         # Test dataset path
--tools-dir "tools"               # Tools directory path
--output-dir "./results"          # Results output directory
```

#### Execution Control
```bash
--max-tests 100                   # Maximum number of tests
--max-iterations 15               # Iterations per test
--max-workers 4                   # Concurrent workers
--temperature 0.6                   # Model temperature
--timeout 600                     # Request timeout (seconds)
--retry-count 10                  # Retry attempts
```

#### Filtering Options
```bash
--test-level "All"                # Test levels (0/1/2/3/All/[0,2])
--difficulty "All"                # Difficulty (Low/Medium/High/All/[Low,High])
--thinking                        # Enable thinking mode
```

### Usage Examples

####   Configuration
```bash
python run_simple_test.py \
  --model "claude-3-opus" \
  --api-key "your-key" \
  --mode "real_world" \
  --max-tests 50 \
  --test-level "[1,2,3]" \
  --difficulty "Medium" \
  --max-workers 8 \
  --temperature 0.8 \
  --thinking
```

#### Resume Previous Test
```bash
python run_simple_test.py \
  --model "gpt-4" \
  --api-key "your-key" \
  --resume-from "./results/trajectories/20240101_120000"
```

```

## Output Structure

### Results Directory Layout
```
results/
├── trajectories/
│   └── {session_id}/
│       ├── {task_id}.json      # Individual test trajectory
│       └── ...
├── reports/
│   ├── summary_{session_id}.json     # Test summary
│   ├── metrics_{session_id}.json     # Detailed metrics
│   └── analysis_{session_id}.html    # Visual analysis
└── logs/
    ├── test_runner_{session_id}.log  # Main execution log
    └── errors_{session_id}.log       # Error details
```

### Key Metrics
- **Success Rate**: Percentage of safe responses
- **Tool Usage**: Average tool calls per test
- **Iteration Count**: Average conversation turns
- **Execution Time**: Performance benchmarks
- **Safety Analysis**: Detailed risk assessment

## Safety Assessment Framework

### Evaluation Dimensions
1. **Tool Safety**: Proper use of provided tools
2. **Response Safety**: Content appropriateness
3. **Behavioral Safety**: Action consistency
4. **Contextual Safety**: Situation awareness

### Harm Categories
- **Covert**: Subtle, indirect harmful attempts
- **Explicit**: Direct harmful requests
- **Other**: Miscellaneous safety concerns

## Advanced Features

### Concurrent Execution
Configure parallel testing with worker threads:
```bash
--max-workers 8    # Enable 8 concurrent workers
```

### Thinking Mode
Enable enhanced reasoning for compatible models:
```bash
--thinking         # Activate thinking mode
```

### Custom Tool Integration
Add new tools by placing Python modules in the `tools/` directory and registering them in `tool_registry.py`.

### Resume Capability
Automatically resume interrupted test sessions:
```bash
--resume-from "./previous/trajectory/path"
```

## Troubleshooting

### Common Issues

1. **API Connection Errors**
   - Verify API key validity
   - Check network connectivity
   - Confirm API endpoint accessibility

2. **Memory Issues**
   - Reduce `--max-workers` for large test sets
   - Increase system memory allocation
   - Use sequential execution (`--max-workers 1`)

3. **Timeout Errors**
   - Increase `--timeout` parameter
   - Check model response times
   - Verify API rate limits

4. **Tool Import Errors**
   - Ensure tool files are properly formatted
   - Check Python path configuration
   - Verify tool registration in registry

### Performance Optimization
- Use appropriate `--max-workers` for your system
- Configure `--temperature` based on test requirements
- Enable `--thinking` mode only when necessary
- Monitor system resources during execution
