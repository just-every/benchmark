# @just-every/benchmark

Comprehensive benchmarking system for @just-every/ensemble that automatically tests models across relevant datasets based on their model class.

[![npm version](https://badge.fury.io/js/@just-every%2Fbenchmark.svg)](https://www.npmjs.com/package/@just-every/benchmark)
[![GitHub Actions](https://github.com/just-every/benchmark/workflows/Release/badge.svg)](https://github.com/just-every/benchmark/actions)

## Overview

The benchmark system provides automated performance testing for LLM models across industry-standard datasets. It intelligently selects appropriate benchmarks based on model capabilities (reasoning, code, vision, etc.) and provides detailed performance metrics including accuracy, latency, and cost analysis.

Perfect for comparing models, tracking performance over time, and making informed decisions about which models to use for specific tasks.

## Features

- 🎯 **Automatic Dataset Selection** - Matches benchmarks to model capabilities
- 📊 **Comprehensive Metrics** - ROUGE scores, F1, exact match, latency stats
- 🏆 **Winner Determination** - Task-specific weighted scoring
- 💰 **Cost Analysis** - Track API costs across providers
- 🔄 **Model Class Support** - Specialized benchmarks for each model type
- 📈 **Detailed Reporting** - Export results to JSON for analysis

## Prerequisites

- Node.js 18.x or higher
- API keys for LLM providers you want to benchmark
- @just-every/ensemble (installed as dependency)

## Environment Setup

Copy `.env.example` to `.env` and add your API keys:

```bash
cp .env.example .env
# Edit .env and add your API keys
```

Supported providers:
- OpenAI (`OPENAI_API_KEY`)
- Anthropic (`ANTHROPIC_API_KEY`)
- Google (`GOOGLE_API_KEY`)
- DeepSeek (`DEEPSEEK_API_KEY`)
- xAI (`XAI_API_KEY`)

## Installation

```bash
npm install
npm run build
```

## Usage

### Primary Usage - Class-Based Benchmarking

The benchmark system automatically selects appropriate datasets based on the model class:

```bash
# Benchmark all models in the 'summary' class
npx tsx src/cli.ts benchmark --class summary

# Benchmark all models in the 'reasoning' class with more samples
npx tsx src/cli.ts benchmark --class reasoning --samples 20

# Test a specific model (even if not in the class)
npx tsx src/cli.ts benchmark --class code --model gpt-4o

# Save results to file
npx tsx src/cli.ts benchmark --class standard --output results/standard-benchmark.json
```

### List Available Model Classes

```bash
npx tsx src/cli.ts list-classes
```

This shows all model classes and their associated datasets.

### Model Class → Dataset Mapping

Each model class is automatically tested on relevant datasets:

- **standard**: MMLU, HellaSwag (general knowledge & reasoning)
- **mini**: Same as standard but with smaller samples
- **reasoning**: GSM8K, ARC Challenge (math & logic)
- **reasoning_mini**: Smaller reasoning datasets
- **code**: HumanEval, MBPP (code generation)
- **writing**: Writing prompts (creative writing)
- **summary**: XSum, CNN/DailyMail (summarization)
- **vision**: VQA, COCO Captions (multimodal)
- **search**: Natural Questions (retrieval-augmented QA)
- And more...

### Available model classes

- `standard` - General purpose models
- `mini` - Small, fast models
- `reasoning` - Models optimized for reasoning tasks
- `reasoning_mini` - Smaller reasoning models
- `code` - Code generation models
- `writing` - Writing and content creation
- `summary` - Summarization models
- `vision` - Vision-capable models
- `vision_mini` - Smaller vision models

### Development mode

```bash
# Run directly with tsx
npx tsx src/cli.ts run --dataset cnn-dailymail --model-class summary

# List available datasets
npx tsx src/cli.ts list
```

## Testing

Run a simple test to ensure ensemble is working:

```bash
npx tsx test.ts
```

## Output

The benchmark will display:
- Performance metrics (ROUGE scores, F1, exact match)
- Latency statistics (average, p95)
- Error rates
- Winner determination based on task-specific weights

Results can be saved to JSON for later analysis:

```bash
npm run benchmark run --dataset squad --model-class qa --output squad-results.json
```

## Example Output

```
📊 Benchmark Results for Model Class: summary

Average Scores Across All Datasets:
┌─────────────────┬────────┬────────┬────────┬────────────┬────────────┐
│ Model           │ rouge1 │ rouge2 │ rougeL │ avgLatency │ p95Latency │
├─────────────────┼────────┼────────┼────────┼────────────┼────────────┤
│ summary         │ 0.245  │ 0.082  │ 0.216  │ 4521ms     │ 5123ms     │
│ gpt-4o-mini     │ 0.221  │ 0.072  │ 0.196  │ 3890ms     │ 4234ms     │
│ claude-3-haiku  │ 0.238  │ 0.079  │ 0.208  │ 2145ms     │ 2456ms     │
│ gemini-1.5-flash│ 0.252  │ 0.085  │ 0.223  │ 3234ms     │ 3567ms     │
└─────────────────┴────────┴────────┴────────┴────────────┴────────────┘

📈 Detailed Results by Dataset:

Dataset: xsum
┌─────────────────┬────────┬────────┬────────┬────────┬─────────┐
│ Model           │ rouge1 │ rouge2 │ rougeL │ Errors │ Duration│
├─────────────────┼────────┼────────┼────────┼────────┼─────────┤
│ summary         │ 0.251  │ 0.089  │ 0.224  │ 0      │ 8.2s    │
│ gpt-4o-mini     │ 0.232  │ 0.078  │ 0.205  │ 0      │ 7.1s    │
└─────────────────┴────────┴────────┴────────┴────────┴─────────┘

🏆 Best Overall Model: gemini-1.5-flash
```

## Development

```bash
# Install dependencies
npm install

# Build TypeScript
npm run build

# Run tests
npm test

# Lint code
npm run lint
```

## Contributing

Contributions are welcome! To add new datasets or improve benchmarking:

1. Fork the repository
2. Create a feature branch
3. Add your dataset in `src/datasets/`
4. Update model class mappings
5. Submit a pull request

## Troubleshooting

### API Key Issues
- Ensure your `.env` file has valid API keys
- Check provider-specific rate limits
- Verify API key permissions

### Performance Issues
- Reduce `--samples` for faster testing
- Use `--model` to test specific models
- Check network connectivity to API endpoints

### Dataset Errors
- Ensure datasets are properly downloaded
- Check dataset format compatibility
- Verify model supports the dataset type

## License

MIT
